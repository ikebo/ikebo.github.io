---
doc_type: weread-highlights-reviews
bookId: "27337546"
author: 伊万·里斯蒂奇
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/546/27337546/t7_27337546.jpg
reviewCount: 1
noteCount: 10
readingStatus: 在读
progress: 6%
totalReadDay: 2
readingTime: 0小时59分钟
readingDate: 2021-11-19
isbn: 9787115432728
lastReadDate: 2021-11-20

---
# 元数据
> [!abstract] HTTPS权威指南：在服务器和Web应用上部署SSL/TLS和PKI
> - ![ HTTPS权威指南：在服务器和Web应用上部署SSL/TLS和PKI|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/546/27337546/t7_27337546.jpg)
> - 书名： HTTPS权威指南：在服务器和Web应用上部署SSL/TLS和PKI
> - 作者： 伊万·里斯蒂奇
> - 简介： 本书是集理论、协议细节、漏洞分析、部署建议于一体的详尽Web应用安全指南。书中具体内容包括：密码学基础，TLS协议，PKI体系及其安全性，HTTP和浏览器问题，协议漏洞；最新的攻击形式，如BEAST、CRIME、BREACH、Lucky 13等；详尽的部署建议；如何使用OpenSSL生成密钥和确认信息；如何使用Apache httpd、IIS、Nginx等进行安全配置。  本书适合Web开发人员、系统管理员和所有对Web应用安全感兴趣的读者。
> - 出版时间 2016-09-12 00:00:00
> - ISBN： 9787115432728
> - 分类： 计算机-理论知识
> - 出版社： 人民邮电出版社
> - PC地址：https://weread.qq.com/web/reader/182328e071a1234a1828fc4

# 高亮划线

### 1.1 传输层安全

> 📌 并提供一个会话缓存方案，以避免这些加密操作在随后的连接中被执行。 
> ⏱ 2021-11-20 02:22:05 ^27337546-9-1432-1464

### 1.2 网络层

> 📌 DNS和BGP就是这样的两个协议。它们同样是不安全的，可以被他人通过各种方式劫持。如果出现这种情况，发往一台计算机的连接可能由攻击者响应。 
> ⏱ 2021-11-20 02:23:34 ^27337546-10-739-808

> 📌 为了避免伪装攻击，SSL和TLS依赖另外一项被称为公钥基础设施（public key infrastructure, PKI）的重要技术，确保将流量发送到正确的接收端。 
> ⏱ 2021-11-20 02:24:26 ^27337546-10-880-997

> 📌 后面的层依次建立在其他层之上，提供更高级别的抽象。最顶层就是应用层，携带着应用数据。 
> ⏱ 2021-11-20 02:25:09 ^27337546-10-1180-1222

### 1.4 密码学

> 📌 填充的每字节都被设置成与填充长度字节相同的值，如图1-3所示。这种方式使得接收方能够检查填充是否正确。 
> ⏱ 2021-11-20 12:11:45 ^27337546-12-7183-7234

> 📌 散列函数（hash function）是将任意长度的输入转化为定长输出的算法。 
> ⏱ 2021-11-20 12:12:12 ^27337546-12-7692-7738

> 📌 但仅在数据的散列与数据本身分开传输的条件下如此。 
> ⏱ 2021-11-20 12:21:15 ^27337546-12-8783-8807

> 📌 加密提供了机密性但无法确保完整性 
> ⏱ 2021-11-20 12:22:21 ^27337546-12-9079-9095

> 📌 HMAC本质就是将散列密钥和消息以一种安全的方式交织在一起 
> ⏱ 2021-11-20 12:29:38 ^27337546-12-9499-9528

> 📌 虽然公钥密码的属性非常有趣，但它却非常缓慢，不适用于数据量大的场景。因此，它往往被部署于进行身份验证和共享秘密的协商，这些秘密后续将用于快速的对称加密。 
> ⏱ 2021-11-20 13:26:35 ^27337546-12-12299-12375

# 读书笔记

## 1.4 密码学

### 划线评论
> 📌 只有拥有散列密钥，才能生成合法的MAC。  ^370982625-7uX2dSPGl
    - 💭 散列密钥，应该就是salt
    - ⏱ 2021-11-20 12:27:08
   
# 本书评论
