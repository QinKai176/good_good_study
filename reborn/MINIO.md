## MINIO

### 1.高可用

Minio采用了`纠删码`技术，即便您丢失一半数量（N/2）的硬盘，您仍然可以恢复数据

### 2.一致性

Minio在分布式和单机模式下，所有读写操作都严格遵守read-after-write一致性模型。这个也难怪，对象存储都是存的比较大的数据，写入耗时比协调耗时要长的多，这就没必要使用类似Raft或者Paxos一样的复杂协调机制；



### 5.参考文章：

https://zhuanlan.zhihu.com/p/170566694

https://avikdas.com/2020/04/13/scalability-concepts-read-after-write-consistency.html

https://blog.csdn.net/bjweimengshu/article/details/80222416

