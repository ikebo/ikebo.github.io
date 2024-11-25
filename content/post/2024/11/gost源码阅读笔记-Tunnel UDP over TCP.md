---
title: Gost源码阅读笔记 Tunnel UDP Over TCP
date: 2024-11-25T17:09:13+08:00
draft: false
---
# Make Tunnel
``` go
func makeTunnel() (c net.Conn, err error) {

	if UseWebsocket || !UseHttp {
	
		c, err = connect(Saddr)
	
	} else {
	
		addr := Saddr
	
		if proxyURL != nil {
	
			addr = proxyURL.Host
	
		}
	
		c, err = dial(addr)
	
	}
	
	if err != nil {
	
		return
	
	}
	
	if UseWebsocket {
	
		ws, resp, err := websocket.NewClient(c, &url.URL{Host: Saddr}, nil, 8192, 8192)
	
		if err != nil {
	
			c.Close()
		
			return nil, err
	
		}
	
		resp.Body.Close()
		
		c = NewWSConn(ws)
	
	} else if UseHttp {
	
		httpcli := NewHttpClientConn(c)
	
		if err = httpcli.Handshake(); err != nil {
		
			c.Close()
	
			return nil, err
	
		}
	
		c = httpcli
	
		//defer httpcli.Close()
	}
	
	sc := gosocks5.ClientConn(c, clientConfig)
	
	if err = sc.Handleshake(); err != nil {
	
		c.Close()
	
		return nil, err
	
	}
	
	c = sc
	
	return
}```



tunnel指的就是一条连接，这条连接传输的数据是其他协议的数据，这里是一条基于tcp的socks5协议连接，将用于传输UDP数据。


# handleSocks5
``` go
func handleSocks5(conn net.Conn, methods []uint8) {
	if err := selectMethod(conn, methods...); err != nil {
		log.Println(err)
		return
	}

	req, err := gosocks5.ReadRequest(conn)
	if err != nil {
		return
	}

	//log.Println(req)
	sconn, err := makeTunnel()
	if err != nil {
		gosocks5.NewReply(gosocks5.Failure, nil).Write(conn)
		log.Println(err)
		return
	}
	defer sconn.Close()

	switch req.Cmd {
	case gosocks5.CmdConnect, gosocks5.CmdBind:
		if err := req.Write(sconn); err != nil {
			return
		}
		Transport(conn, sconn)
	case gosocks5.CmdUdp:
		if err := req.Write(sconn); err != nil {
			return
		}
		rep, err := gosocks5.ReadReply(sconn)
		if err != nil || rep.Rep != gosocks5.Succeeded {
			return
		}

		uconn, err := net.ListenUDP("udp", nil)
		if err != nil {
			log.Println(err)
			gosocks5.NewReply(gosocks5.Failure, nil).Write(conn)
			return
		}
		defer uconn.Close()

		addr := ToSocksAddr(uconn.LocalAddr())
		addr.Host, _, _ = net.SplitHostPort(conn.LocalAddr().String())
		log.Println("udp:", addr)

		rep = gosocks5.NewReply(gosocks5.Succeeded, addr)
		if err := rep.Write(conn); err != nil {
			log.Println(err)
			return
		}

		go cliTunnelUDP(uconn, sconn)

		// block, waiting for client exit
		ioutil.ReadAll(conn)
	}
}
```


与Server建立连接后 (makeTunnel)，本地listen udp, 将udp连接(uconn) 和 server连接(sconn) pipe起来, 也就是将udp协议数据通过sconn这个tunnel转发出去, tunnel对端是个socks server, socks5协议本身就支持udp, 会将tcp协议中的udp数据解包, 再转发到对应的目标地址。

 
 以下是pipe的具体实现：
# cliTunnelUDP 
``` go
func cliTunnelUDP(uconn *net.UDPConn, sconn net.Conn) {
	var raddr *net.UDPAddr

	go func() {
		b := lpool.Take()
		defer lpool.put(b)

		for {
			n, addr, err := uconn.ReadFromUDP(b)
			if err != nil {
				log.Println(err)
				return
			}
			raddr = addr
			r := bytes.NewBuffer(b[:n])
			udp, err := gosocks5.ReadUDPDatagram(r)
			if err != nil {
				return
			}
			udp.Header.Rsv = uint16(len(udp.Data))
			//log.Println("r", raddr.String(), udp.Header)

			if err := udp.Write(sconn); err != nil {
				log.Println(err)
				return
			}
		}
	}()

	for {
		b := lpool.Take()
		defer lpool.put(b)

		udp, err := gosocks5.ReadUDPDatagram(sconn)
		if err != nil {
			log.Println(err)
			return
		}
		//log.Println("w", udp.Header)
		udp.Header.Rsv = 0
		buf := bytes.NewBuffer(b[0:0])
		udp.Write(buf)
		if _, err := uconn.WriteTo(buf.Bytes(), raddr); err != nil {
			log.Println(err)
			return
		}
	}
}
```

