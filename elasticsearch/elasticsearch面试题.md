## ElasticSearch

### 版本1

1、elasticsearch 了解多少，说说你们公司 es 的集群架构，索引数据大小，分片有多少，以及一些调优手段 。

2、elasticsearch 的倒排索引是什么

3、elasticsearch 索引数据多了怎么办，如何调优，部署

4、elasticsearch 是如何实现 master 选举的

5、详细描述一下 Elasticsearch 索引文档的过程

6、详细描述一下 Elasticsearch 搜索的过程？

7、Elasticsearch 在部署时，对 Linux 的设置有哪些优化方法

8、lucence 内部结构是什么？

9、Elasticsearch 是如何实现 Master 选举的？

10、Elasticsearch 中的节点（比如共 20 个），其中的 10 个选了一个master，另外 10 个选了另一个 master，怎么办？

11、客户端在和集群连接时，如何选择特定的节点执行请求的？

12、详细描述一下 Elasticsearch 索引文档的过程。

13、详细描述一下 Elasticsearch 更新和删除文档的过程。

14、详细描述一下 Elasticsearch 搜索的过程。

15、在 Elasticsearch 中，是怎么根据一个词找到对应的倒排索引的？

16、Elasticsearch 在部署时，对 Linux 的设置有哪些优化方法？

17、对于 GC 方面，在使用 Elasticsearch 时要注意什么？

18、Elasticsearch 对于大数据量（上亿量级）的聚合如何实现？

19、在并发情况下，Elasticsearch 如果保证读写一致？

20、如何监控 Elasticsearch 集群状态？

21、介绍下你们电商搜索的整体技术架构。

22、介绍一下你们的个性化搜索方案？

23、是否了解字典树？

24、拼写纠错是如何实现的？

### 版本2

1.什么是倒排索引，什么是全文检索？

这种先建立索引，再对索引进行搜索的过程就叫全文检索(Full-text Search) 。

倒排索引：之前是先建立index，再搜索到目标；现在是先提取关键字作为索引，再得到结果；

