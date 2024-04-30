---
title: "MySQL InnoDB 行记录格式(ROW_FORMAT)"
date: 2024-04-30T11:48:10+08:00
draft: false
---
**MySQL InnoDB 行记录格式（ROW_FORMAT）**

**一、行记录格式的分类和介绍**

在早期的InnoDB版本中，由于文件格式只有一种，因此不需要为此文件格式命名。随着InnoDB引擎的发展，开发出了不兼容早期版本的新文件格式，用于支持新的功能。为了在升级和降级情况下帮助管理系统的兼容性，以及运行不同的MySQL版本，InnoDB开始使用命名的文件格式。

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180807110611327-1802656088.png)

1\. Antelope: 先前未命名的，原始的InnoDB文件格式。它支持两种行格式：COMPACT 和 REDUNDANT。MySQL5.6的默认文件格式。可以与早期的版本保持最大的兼容性。不支持 Barracuda 文件格式。

2\. Barracuda: 新的文件格式。它支持InnoDB的所有行格式，包括新的行格式：COMPRESSED 和 DYNAMIC。与这两个新的行格式相关的功能包括：InnoDB表的压缩，长列数据的页外存储和索引建前缀最大长度为3072字节。

在 msyql 5.7.9 及以后版本，默认行格式由`innodb_default_row_format`变量决定，它的默认值是`DYNAMIC`，也可以在 create table 的时候指定`ROW_FORMAT=DYNAMIC`。用户可以通过命令 `SHOW TABLE STATUS LIKE'table_name'` 来查看当前表使用的行格式，其中 *row_format* 列表示当前所使用的行记录结构类型。

PS：如果要修改现有表的行模式为`compressed`或`dynamic`，必须先将文件格式设置成Barracuda：`set global innodb_file_format=Barracuda;`，再用`ALTER TABLE tablename ROW_FORMAT=COMPRESSED;`去修改才能生效。

```
mysql> show variables like "innodb_file_format"; +--------------------+-----------+
| Variable_name      | Value     |
+--------------------+-----------+
| innodb_file_format | Barracuda |
+--------------------+-----------+
1 row in set (0.00 sec)
```

```
mysql> show table status like "test%"\G *************************** 1. row *************************** Name: test
         Engine: MyISAM
        Version: 10 Row_format: Dynamic
           Rows: 4 Avg_row_length: 20 Data_length: 80 Max_data_length: 281474976710655 Index_length: 1024 Data_free: 0 Auto_increment: NULL Create_time: 2018-08-07 13:07:59 Update_time: 2018-08-07 13:08:01 Check_time: NULL Collation: utf8_general_ci
       Checksum: NULL Create_options: row_format=DYNAMIC
        Comment:
```

二、InnoDB行存储

InnoDB表的数据存储在页（page）中，每个页可以存放多条记录。这些页以树形结构组织，这颗树称为B树索引。表中数据和辅助索引都是使用B树结构。维护表中所有数据的这颗B树索引称为聚簇索引，通过主键来组织的。聚簇索引的叶子节点包含行中所有字段的值，辅助索引的叶子节点包含索引列和主键列。

变长字段是个例外，例如对于BLOB和VARCHAR类型的列，当页不能完全容纳此列的数据时，会将此列的数据存放在称为溢出页(overflow page)的单独磁盘页上，称这些列为页外列(off-page column)。这些列的值存储在以单链表形式存在的溢出页列表中，每个列都有自己溢出页列表。某些情况下，为了避免浪费存储空间和消除读取分隔页，列的所有或前缀数据会存储在B+树索引中。

**三、Compact 和 Redundant**

（一）Compact

Compact行记录是在MySQL5.0中引入的，为了高效的存储数据，简单的说，就是为了让一个页（Page）存放的行数据越多，这样性能就越高。行记录格式如下：

 　　![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808114049329-828726055.png)

1\. 变长字段长度列表：变长字段长度最大不超过2字节（MySQL数据库varcahr类型的最大长度限制为65535）

2\. NULL标识位：该位指示了该行数据中是否有NULL值，有则用1。

3\. 记录头信息：固定占用5字节（40位）

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808135251106-8868400.png)

4\. 列N数据：实际存储每列的数据，NULL不占该部分任何空间，即NULL占有NULL标志位，实际存储不占任何空间。

PS：每一行数据除了用户定义的例外，还有两个隐藏列，事物ID列和回滚指针列，分别位6字节和7字节的大小，若InnoDB表没有定义主键，每行还未增加一个6字节的rowid列。

（二）Redundant

MySQL5.0之前的行记录格式：

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808134937833-353015839.png)

1\. 字段偏移列表：同样是按照列的顺序逆序放置的，若列的长度小于255字节，用1字节表示，若大于255字节，用2字节表示。

2\. 记录头信息：占用6字节（48位）

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808135218891-1002075113.png)

（三）行溢出数据

1\. 当`行记录的长度`没有超过`行记录最大长度`时，`所有数据`都会存储在`当前页。`

2\. 当`行记录的长度`超过`行记录最大长度`时，变长列（`variable-length column`）会选择外部溢出页（`overflow page`，一般是`Uncompressed BLOB Page`）进行存储。

`Compact` + `Redundant`：保留前`768Byte`在当前页（`B+Tree叶子节点`），其余数据存放在`溢出页`。`768Byte`后面跟着`20Byte`的数据，用来存储`指向溢出页的指针。`

（四）概述

对于 Compact 和 Redundant 行格式，InnoDB将变长字段(VARCHAR, VARBINARY, BLOB 和 TEXT)的前786字节存储在B+树节点中，其余的数据存放在溢出页(off-page)，如下图：

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808135918742-634619223.png)

上面所讲的讲的blob或变长大字段类型包括blob,text,varchar，其中varchar列值长度大于某数N时也会存溢出页，在latin1字符集下N值可以这样计算：innodb的块大小默认为16kb，由于innodb存储引擎表为索引组织表，树底层的叶子节点为一双向链表，因此每个页中至少应该有两行记录，这就决定了innodb在存储一行数据的时候不能够超过8k，减去其它列值所占字节数，约等于N。

使用Antelope文件格式，若字段的值小于等于786字节，不需要溢出页，因为字段的值都在B+树节点中，所以会降低I/O操作。这对于相对较短的BLOB字段有效，但可能由于B+树节点存储过多的数据而导致效率低下。

 四、Compressed 和 Dynamic

InnoDB1.0x开始引入心的文件格式（file format，用户可以理解位新的页格式）------Barracuda（图1），这个新的格式拥有两种新的行记录格式：Compressed和Dynamic。

新的两种记录格式对于存放BLOB中的数据采用了完全的行溢出的方式。如图：

![](https://images2018.cnblogs.com/blog/1062001/201808/1062001-20180808141739876-1872305389.png)

Dynamic行格式，列存储是否放到off-page页，主要取决于行大小，他会把行中最长的一列放到off-page，直到数据页能存放下两行。TEXT或BLOB列<=40bytes时总是存在于数据页。这种方式可以避免compact那样把太多的大列值放到B-tree Node（数据页中只存放20个字节的指针，实际的数据存放在Off Page中，之前的Compact 和 Redundant 两种格式会存放768个字前缀字节）。

Compressed物理结构上与Dynamic类似，Compressed行记录格式的另一个功能就是存储在其中的行数据会以zlib的算法进行压缩，因此对于BLOB、TEXT、VARCHAR这类大长度数据能够进行有效的存储（减少40%，但对CPU要求更高）。

 **若有不恰当之处，还望指教，谢谢！**

参考
--

1\. 《MySQL技术内幕-InnoDB存储引擎》

2\. [《InnoDB备忘录-行记录格式》](http://zhongmingmao.me/2017/05/07/innodb-table-row-format/)

3\. 《[MySQL 大字段溢出导致数据回写失败](http://blog.opskumu.com/mysql-blob.html)》


转载自[博客园](https://www.cnblogs.com/wilburxu/p/9435818.html)