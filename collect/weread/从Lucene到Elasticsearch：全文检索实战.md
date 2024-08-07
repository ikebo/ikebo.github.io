---
doc_type: weread-highlights-reviews
bookId: "26943367"
author: 姚攀
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/367/26943367/t7_26943367.jpg
reviewCount: 1
noteCount: 87
readingStatus: 读完
progress: 99%
totalReadDay: 4
readingTime: 4小时54分钟
readingDate: 2021-12-16
finishedDate: 2021-12-22
isbn: 9787302483069
lastReadDate: 2021-12-22

---
# 元数据
> [!abstract] 从Lucene到Elasticsearch：全文检索实战
> - ![ 从Lucene到Elasticsearch：全文检索实战|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/367/26943367/t7_26943367.jpg)
> - 书名： 从Lucene到Elasticsearch：全文检索实战
> - 作者： 姚攀
> - 简介： 本书循序渐进介绍了信息检索、布尔检索、向量空间模型、tf-idf、BM25排序算法、Lucene架构、Lucene创建索引、Lucene查询、Lucene项目实战、Elasticsearch安装与配置、Elasticsearch插件安装、REST API数据操作、映射与模板、索引别名、Elasticsearch基本和高级搜索、Elasticsearch同步数据库、Elasticsearch集群管理、项目实战等内容。阅读本书，读者能够掌握信息检索的核心概念，应用Lucene库处理全文检索业务，掌握Elasticsearch分布式搜索引擎的使用方法与技巧。 本书基于Lucene 6.0和Elasticsearch 5.4.0进行讲解，技术先进，示例丰富，适合想学习信息检索技术的初学者和相关专业的大学生、研究生学习，也很适合大数据及云计算平台构建人员以及有一定基础的IT开发人员使用。
> - 出版时间 2017-12-01 00:00:00
> - ISBN： 9787302483069
> - 分类： 计算机-数据库
> - 出版社： 清华大学出版社
> - PC地址：https://weread.qq.com/web/reader/f2132080719b1f87f2167b1

# 高亮划线

## 前言

> 📌 入门搜索技术并不困难 
> ⏱ 2021-12-19 21:35:22 ^26943367-3-904-914

### 1.3 倒排索引

> 📌 它是文档检索系统中最常用的数据结构。 
> ⏱ 2021-12-19 21:52:58 ^26943367-7-959-977

### 1.4 布尔检索模型

> 📌 检索模型是判断文档内容与用户查询相关性的核心技术，以大规模网页搜索为例，在海量网页中与用户查询关键词相关的网页可能会有成千上万个，甚至更多。 
> ⏱ 2021-12-19 21:55:40 ^26943367-8-425-495

> 📌 文档2和文档4是符合查询条件的结果。 
> ⏱ 2021-12-19 21:59:11 ^26943367-8-2248-2266

> 📌 易于实现，这也是现在的各种检索系统中都提供布尔检索的重要原因。 
> ⏱ 2021-12-19 21:59:41 ^26943367-8-2595-2626

> 📌 第一，它的检索策略只基于0和1二元判定标准。 
> ⏱ 2021-12-19 21:59:55 ^26943367-8-2694-2716

> 📌 缺乏文档分级（rank）的概念，不能进行关键词重要性排序，限制了检索功能。 
> ⏱ 2021-12-19 22:00:04 ^26943367-8-2736-2773

> 📌 完全匹配会导致太少的结果文档被返回。没有加权的概念，容易出现漏检。 
> ⏱ 2021-12-19 22:00:25 ^26943367-8-2893-2926

### 1.5 tf-idf权重计算

> 📌 分母中文档数加1是进行平滑处理，防止所有文档都不包含某个词时分母为0的情况发生 
> ⏱ 2021-12-19 22:03:02 ^26943367-9-1652-1691

### 1.6 向量空间模型

> 📌 推荐系统中可以用来比较用户偏好的相似性 
> ⏱ 2021-12-19 22:18:28 ^26943367-10-3745-3764

### 1.7 概率检索模型

> 📌 叶斯决策理论在机器学习、自然语 
> ⏱ 2021-12-19 22:45:43 ^26943367-11-2324-2339

> 📌 处理等领域被广泛应用，在海量数据的文本分类问题（比如垃圾邮件、垃圾短信的甄别和过滤）上能取得非常好的效果，其核心思想是选择高概率对应的类别。
以图1-3为例，数据集中有实心圆和空心圆两 
> ⏱ 2021-12-19 22:45:47 ^26943367-11-2340-2460

### 2.1 Lucene概述

> 📌 资深全文检索专家Doug Cutting用一个周末的时间，使用Java语言创作了一个文本搜索的开源函数库， 
> ⏱ 2021-12-20 09:01:38 ^26943367-14-554-607

> 📌 维基百科用Lucene建立了一个站内的强大搜索功能 
> ⏱ 2021-12-20 09:02:10 ^26943367-14-929-954

> 📌 索引的大小约为索引文本大小的20%～30%。 
> ⏱ 2021-12-20 09:02:50 ^26943367-14-1293-1315

> 📌 许多强大的查询类型：短语查询、通配符查询、近似查询、范围查询等。 
> ⏱ 2021-12-20 09:03:03 ^26943367-14-1446-1478

> 📌 对字段级别搜索（如标题，作者，内容）。 
> ⏱ 2021-12-20 09:03:10 ^26943367-14-1510-1529

> 📌 支持搜索多个索引并合并搜索结果。 
> ⏱ 2021-12-20 09:03:21 ^26943367-14-1603-1619

> 📌 灵活的切面、高亮、join和group by功能。 
> ⏱ 2021-12-20 09:03:39 ^26943367-14-1699-1724

> 📌 采集万维网上的信息一般使用网络爬虫。完成信息采集之后到Lucene层面主要有两大任务： 
> ⏱ 2021-12-20 09:04:59 ^26943367-14-2578-2621

> 📌 索引文档的过程完成由原始文 
> ⏱ 2021-12-20 09:06:11 ^26943367-14-2631-2644

> 📌 档到倒排索引的构建过程， 
> ⏱ 2021-12-20 09:06:24 ^26943367-14-2644-2656

> 📌 ，经过分词、匹配、评分、排序等一系列过程之后返回用户想要的文档。 
> ⏱ 2021-12-20 09:06:39 ^26943367-14-2708-2740

> 📌 倒排索引就是关键词和文档之间的对应关系，就像给文档贴上标签。 
> ⏱ 2021-12-20 09:08:25 ^26943367-14-3666-3696

### 2.5 Lucene查询详解

> 📌 缀搜索（PrefixQuery） 
> ⏱ 2021-12-20 23:22:28 ^26943367-18-7814-7830

### 2.8 本章小结

> 📌 本章的每个示例都给出了代码和运行结果，“纸上得来终觉浅，绝知此事要躬行”，希望读者能够动手实践，掌握Lucene基础。 
> ⏱ 2021-12-20 23:34:07 ^26943367-21-470-529

### 3.2 架构设计

> 📌 使用开源工具Tika完成信息抽取，使用Lucene构建索引，使用JSP页面给用户提供查询接口，使用Servlet完成搜索，构建类百度文库的小型文件检索系统。 
> ⏱ 2021-12-20 23:36:31 ^26943367-24-855-934

### 3.3 文本内容抽取

> 📌 内容解析提取工具Apache Tika。 
> ⏱ 2021-12-20 23:36:50 ^26943367-25-483-563

> 📌 Apache Tika是一个用于文件类型检测和文件内容提取的库，该项目于2007年3月开始启动，最开始是Apache Lucene项目的子项目，2010年5月成为Apache组织的顶级项目，该项目的目标使用群体主要为搜索引擎以及其他内容索引和分析工具，编程语言为Java。 
> ⏱ 2021-12-20 23:37:23 ^26943367-25-605-741

> 📌 Tika广泛应用于搜索引擎、内容分析、文本翻译、数字资产管理等诸多领域。 
> ⏱ 2021-12-20 23:37:48 ^26943367-25-806-842

> 📌 灵活元数据。Tika理解所有用来描述文件的元数据模型。 
> ⏱ 2021-12-20 23:39:09 ^26943367-25-1354-1381

### 4.1 Elasticsearch概述

> 📌 日志采集与解析工具Logstash 
> ⏱ 2021-12-20 23:57:42 ^26943367-33-2072-2089

> 📌 可视化分析平台Kibana 
> ⏱ 2021-12-20 23:57:46 ^26943367-33-2090-2103

> 📌 非常流行的集中式日志解决方案 
> ⏱ 2021-12-21 00:00:58 ^26943367-33-2176-2190

> 📌 Filebeat轻量级的日志采集器，可用于收集文件数据。 
> ⏱ 2021-12-21 00:02:01 ^26943367-33-2470-2498

> 📌 Gateway是Elasticsearch用来存储索引的文件系统，支持多种文件类型，Local FileSystem是存储在本地的文件系统，Shared FileSystem是共享存储，也可以使用Hadoop的HDFS分布式存储，也可以存储在Amazon S3云服务上。 
> ⏱ 2021-12-21 00:05:11 ^26943367-33-4289-4424

> 📌 Elasticsearch的底层API是由Lucene提供的，每一个Elasticsearch节点上都有一个Lucene引擎的支持。 
> ⏱ 2021-12-21 00:05:38 ^26943367-33-4479-4545

> 📌 Scripting用来支持JavaScript、Python等多种语言，可以在查询语句中嵌入，使用Script语句性能稍低。Elasticsearch也支持多种第三方插件。 
> ⏱ 2021-12-21 00:06:07 ^26943367-33-4836-4922

> 📌 Elasticsearch的自动发现机制会识别新增的节点并重新平衡分配数据。
● 全文检索：Apache 
> ⏱ 2021-12-21 00:07:04 ^26943367-33-5689-5770

> 📌 近实时搜索和分析：数据从进入Elasticsearch，可达到近实时搜索。除了搜索，Elasticsearch也可以进行聚合分析操作。 
> ⏱ 2021-12-21 00:07:31 ^26943367-33-5925-5992

> 📌 Elasticsearch集群会自动发现新的或失败的节点，重组和重新平衡数据，确保数据是安全的和可访问的。 
> ⏱ 2021-12-21 00:07:39 ^26943367-33-6042-6095

> 📌 Elasticsearch在读写性能上优于MongoDB，同时也支持地理位置查询。 
> ⏱ 2021-12-21 00:08:13 ^26943367-33-6911-6952

> 📌 日志分析由实时日志分析平台ELK（ELK由Elasticsearch、Logstash和Kiabana三个开源工具组成）完成 
> ⏱ 2021-12-21 00:08:31 ^26943367-33-7020-7082

> 📌 进行集中的收集、存储、搜索、分析、监控以及可视化。 
> ⏱ 2021-12-21 00:08:44 ^26943367-33-7088-7113

> 📌 使用Elasticsearch来处理访客日志，以便能将公众对不同文章的反应实时地反馈给各位编辑。 
> ⏱ 2021-12-21 00:09:18 ^26943367-33-7424-7472

> 📌 GitHub使用Elasticsearch来检索超过1300亿行代码。 
> ⏱ 2021-12-21 00:09:31 ^26943367-33-7504-7539

> 📌 SoundCloud是一家德国网站，提供音乐分享社区服务，使用Elasticsearch来为1.8亿用户提供即时精准的音乐搜索服务。 
> ⏱ 2021-12-21 00:09:45 ^26943367-33-7571-7637

> 📌 测试的结果以JSON的方式索引到Elasticsearch中，开发人员可以非常方便地查找bug 
> ⏱ 2021-12-21 00:10:42 ^26943367-33-7715-7762

> 📌 Socorro是Mozilla公司的程序崩溃报告系统，一有错误信息就插入到Hbase和Postgres中，然后从Hbase中读取数据索引到Elasticsearch中，方便查找。 
> ⏱ 2021-12-21 00:11:03 ^26943367-33-7763-7852

> 📌 。通常，会为具有一组共同字段的文档定义一个类型。 
> ⏱ 2021-12-21 09:52:59 ^26943367-33-9663-9687

> 📌 文档都是JSON格式。 
> ⏱ 2021-12-21 09:53:14 ^26943367-33-9818-9829

> 📌 为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫作分片。 
> ⏱ 2021-12-21 09:53:57 ^26943367-33-9980-10026

> 📌 ，每个分片本身也是一个功能完善并且独立的“索引” 
> ⏱ 2021-12-21 09:54:10 ^26943367-33-10050-10074

> 📌 ，这个“索引”可以被放置到集群中的任何节点上。 
> ⏱ 2021-12-21 09:54:15 ^26943367-33-10074-10097

> 📌 允许你在分片（潜在地，位于多个节点上）上进行分布式的、并行的操作，进而提高性能和吞吐量，至于一个分片怎样分布， 
> ⏱ 2021-12-21 09:54:51 ^26943367-33-10227-10282

> 📌 为此，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫作复制分片，或者直接叫副本。 
> ⏱ 2021-12-21 09:55:19 ^26943367-33-10481-10532

> 📌 。因为这个原因，复制分片不与主分片置于同一节点上，这一点非常重要。 
> ⏱ 2021-12-21 09:55:29 ^26943367-33-10630-10663

> 📌 搜索可以在所有的副本上并行运行。 
> ⏱ 2021-12-21 09:55:40 ^26943367-33-10709-10725

> 📌 每个索引就有了主分片（作为复制源的原来的分片）和副本分片（主分片的拷贝）之别 
> ⏱ 2021-12-21 09:55:55 ^26943367-33-10792-10830

> 📌 在索引创建之后，可以在任何时候动态地改变副本的数量，但是事后不能改变分片的数量。 
> ⏱ 2021-12-21 09:56:07 ^26943367-33-10852-10892

### 5.2 文档管理

> 📌 一个常见的场景是使用关系型数据库做主存储，使用Elasticsearch做检索引擎， 
> ⏱ 2021-12-21 23:18:14 ^26943367-41-14682-14724

> 📌 因关系型数据库中有保护数据一致性的机制 
> ⏱ 2021-12-21 23:18:24 ^26943367-41-14724-14743

> 📌 但在要求数据强一致性的业务中缺少并发控制就会带来隐患，仍然需要控制。 
> ⏱ 2021-12-21 23:18:43 ^26943367-41-14822-14856

> 📌 如果有线程对数据进行修改就对数据进行锁定，其他线程想要访问需要等待当前锁定释放，这样可确保同一时刻最多只有一个线程访问数据。传统的关系型数据库就用到了很多这种锁机制，比如行锁、表锁、读锁、写锁等 
> ⏱ 2021-12-21 23:20:42 ^26943367-41-15079-15176

> 📌 假定不会发生并发访问冲突，对数据资源不会锁定，只有在数据提交操作时检查是否违反数据完整性。 
> ⏱ 2021-12-21 23:21:24 ^26943367-41-15276-15321

> 📌 Elasticsearch使用的就是乐观锁机制，乐观锁适用于读操作比较多的应用类型，可省去锁开销，可以提高吞吐量。 
> ⏱ 2021-12-21 23:21:30 ^26943367-41-15321-15378

> 📌 新版本的文档必须要复制到集群中的其他节点 
> ⏱ 2021-12-21 23:22:42 ^26943367-41-15442-15462

> 📌 那么Elasticsearch是如何知道文档属于哪个分片的呢 
> ⏱ 2021-12-22 00:01:07 ^26943367-41-17279-17309

### 5.3 映射详解

> 📌 在实际项目中，如果遇到的业务在导入数据之前不确定有哪些字段，也不清楚字段的类型是什么，使用动态映射非常合适。 
> ⏱ 2021-12-22 00:05:52 ^26943367-42-1307-1361

> 📌 排序、聚合。类型为keyword的字段只能通过精确值搜索到，区别于text类型 
> ⏱ 2021-12-22 00:10:27 ^26943367-42-7462-7501

> 📌 _all字段会被解析和索引，但是不存储。当需要返回包含某个关键字的文档，但是不明确地搜某个字段的时候，可以对_all字段进行搜索。例子如下： 
> ⏱ 2021-12-22 00:35:36 ^26943367-42-27524-27594

> 📌 _field_names字段用来存储文档中的所有非空字段的名字，这个字段常用于exists查询。例子如下： 
> ⏱ 2021-12-22 00:36:57 ^26943367-42-28099-28152

> 📌 搜索具有body字段的文档：
        GET my_index/_search￼         {￼           "query": {￼             "terms": {￼               "_field_names": [ "body" ]￼             }￼           }￼         }
结果会返回第二条文档，因为第一条文档只有title字段。 
> ⏱ 2021-12-22 00:36:38 ^26943367-42-28453-28707

> 📌 额外增加一个列式存储映射 
> ⏱ 2021-12-22 00:39:32 ^26943367-42-36733-36745

> 📌 包含查询关键词的文档有哪些？”，聚合恰恰相反，聚合要解决的问题是“文档包含哪些词项”， 
> ⏱ 2021-12-22 00:40:32 ^26943367-42-38249-38292

### 6.1 搜索机制

> 📌 Elasticsearch的核心功能是搜索 
> ⏱ 2021-12-22 10:28:54 ^26943367-45-482-503

> 📌 一份是该文档的原始内容，也就是_source中的文档内容，另一份是索引时通过分词、过滤等一系列过程生成的倒排索引文件，倒排索引中保存了词项和文档的对应关系。 
> ⏱ 2021-12-22 10:30:16 ^26943367-45-1091-1169

> 📌 ，然后做评分、排序、高亮处理，最终返回搜索结果给用户。 
> ⏱ 2021-12-22 10:30:34 ^26943367-45-1282-1309

> 📌 搜索机制解决的是相关度的问题， 
> ⏱ 2021-12-22 10:30:49 ^26943367-45-1338-1353

> 📌 文档和查询关键词之间的相关度，按照评分排序后返回最相关的文档给用户。 
> ⏱ 2021-12-22 10:31:20 ^26943367-45-1384-1418

> 📌 另外，Elasticsearch中还有一种过滤机制，过滤机制解决的是只根据条件对文档进行过滤，不计算评分。 
> ⏱ 2021-12-22 10:31:28 ^26943367-45-1418-1471

### 6.4 复合查询

> 📌 可返回与任意查询条件子句匹配的任何文档类型。 
> ⏱ 2021-12-22 17:42:23 ^26943367-48-2263-2285

### 7.1 指标聚合

> 📌 Percentiles Aggregation 
> ⏱ 2021-12-22 17:51:14 ^26943367-56-5297-5320

### 7.2 桶聚合

> 📌 Terms Aggregation用于分组聚合。 
> ⏱ 2021-12-22 17:52:12 ^26943367-57-722-746

> 📌 Filter Aggregation是过滤器聚 
> ⏱ 2021-12-22 17:53:03 ^26943367-57-2992-3015

### 9.2 索引规划

> 📌 我们知道分片是把一个大的索引分成多份放到不同的节点上来加速查询效率 
> ⏱ 2021-12-22 18:20:56 ^26943367-72-480-513

### 9.3 分布式集群

> 📌 client节点：client节点起到路由请求的作用，实际上可以看作负载均衡器 
> ⏱ 2021-12-22 18:25:38 ^26943367-73-704-743

## 参考文献

> 📌  
> ⏱ 2021-12-22 18:56:07 ^26943367-92-2179

# 读书笔记

# 本书评论

## 书评 No.1 
不错，讲的比较清晰全面，适合入门  ^370982625-7vK7ALy0t
⏱ 2021-12-22 18:55:59
