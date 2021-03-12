## NEXT

### 1.Step1

Java 工程师面试突击第 1 季-中华石杉老师

springboot的启动机制

bilibili  springsecurity视频

rabbitmq  江南一点雨

ansible

### 2.chapter2

二、准备过程 + 复习计划
开始有跳槽的想法是在20年3月，公司进行了大规模裁员，可能我作为相对核心的开发，两轮都没有裁到我。
这时候实际开始有居安思危的想法，想着对实习 + 工作阶段的东西进行整理，万一出什么事不用再开始忙。
这里主要分了三大步，并给出我自己的例子用来参考：

A、基础梳理：2020.3 - 2020.8
先上Boss等平台梳理要求，以及自己的技术栈；按照知识点梳理，思考业务中的使用；对源码的思考；制作复习笔记。

举例1----MySQL：
1、通过书籍 / 文章梳理出知识点 ：例如 MySQL技术内幕、极客时间
2、思考自己业务上对知识的应用，并延展开。
例如：
为什么要通用InnoDB引擎？
哪些情况可以用MyIsam呢？
行锁这一块可以用在哪些场景？
limit如何实现的？
B树、红黑树对B+树的缺点？
B+树对计算机系统层面上来说有什么优点？
MVCC如何实现的？对业务上会出现哪些问题？

举例2----Dubbo：
1、准备：先过了一遍官网的文档 & 示例。重点读了一本深入理解Apache Dubbo。
2、思考：
画一遍Dubbo的架构设计图
画一遍dubbo注册和请求的流程图。
Dubbo IO线程和通信线程的交互图。
服务治理时的原理，以及timeout部分源码
注册中心如何实现的。
Dubbo 如何和Spring打通的？
Dubbo SPI的优点和实现原理。业务中如何使用SPI这种方式做业务呢？等等

这个过程算是很漫长，一定要坚持写笔记，坚持想，坚持看，哪怕是错的也没关系，慢慢水平上来之后再纠错再反思。

B、尝试阶段 2020.8-2020.9
这里主要花了一个月时间去面试，有大公司有小公司，也拿到了相对当时工资来说还不错的offer，最终没有走，因为定的目标就是尝试。
主要目标是总结面试，对面的不好的地方进行整理，再提升。


C、冲刺阶段 2020.10-2020.12
赶上公司双11、12， 也算是一边作为PM带小项目，一遍继续整理知识。
这里算是比较好的机会，总结了很多关于公司的项目目标、规划、项目流程以及管理的经验。
最终算是取到了不错的效果。

总结：

坚持学习，保持热情，相信自己，不要灰心。





三、各种学习资料推荐

1、公众号：
我对现在的公众号其实比较反感，
一是很多人都是在贩卖焦虑
二是很多人都是在刷重复的东西。。想象一下早上打开微信，十几个公众号推一样东西的绝望。。。。
三讨厌标题狗。。。

给出几个我感觉相对有干货的公众号，他们也要恰饭，对于一些贩卖焦虑和标题的文章，就当没看见吧。。。。也算是矮子拔高个了
排名不分先后哦 ， 按照微信公众号列表的顺序：
程序员DD 、 Golang语言社区 、 GitHubDaily、 Hollis 、架构师之路、 路人甲Java等

2、git项目、文章 & 网站
https://github.com/doocs

2 Java多线程入门类和接口  多线程社区

Java工程师成神之路

LeetCode、剑指Offer、程序员面试金典题解 

题库 - 力扣 (LeetCode)

Snailclimb/guide-rpc-framework

免费在线学习代码重构和设计模式

Data Structure Visualization  数据结构演示 非常好的

怎么学习golang？- Go语言中文网 - Golang中文社区

简明 Vim 练级攻略 | | 酷 壳 - CoolShell  vim

CL0610/Java-concurrency  java并发总结

3、书籍 & 专栏等 推荐
《大话数据结构》

《大话设计模式》
《Head First设计模式》

《深入理解Java虚拟机》
《深入理解Java内存模型》

《极客时间-mysql45讲》
《MySQL技术内幕-InnoDB引擎》
《高性能MySQL》
《MyBatis技术内幕》

《深入理解Apache Dubbo》

《RocketMQ技术内幕：RocketMQ架构设计与实现原理》

Spring官方文档

《Redis实战》
《Redis设计与实现》
Redis源码
《Redis深度历险：核心原理与应用实践》

《啊哈算法》
《Java8实战》
《Java8编程思想》

《Java多线程编程核心技术》
《linux命令行与shell脚本编程》
《Git权威指南》

《阿里巴巴开发手册-华山版》

4、生产力工具

测试：

charles 种cookie 
switchHost 改本地host
postMan  测试接口

笔记：
Notion 超赞
mac 备忘录
Typora  
Background music

开发：
Alfred  可以自己写脚本实现很多有意思的小功能
Intellj IDEA
PhpStorm
PyCharm
DataGrip
VsCode
Sublime text
Iterm + Oh my zsh





# [springboot集成ElasticApm](https://www.cnblogs.com/wujf/p/13743910.html)