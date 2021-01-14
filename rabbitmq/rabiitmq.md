##  Rabbitmq

### 1.安装使用

```
docker pull rabbitmq:3.7.7-management
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=my_vhost  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin df80af9ca0c9
```

### 2.rabbit的api

![l2UxEv](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/l2UxEv.png)

### 3.问题

##  前言

- 你用过消息队列么？
- 说说你们项目里是怎么用消息队列的？
  - 我们有一个订单系统，订单系统会每次下一个新订单的时候，就会发送一条消息到ActiveMQ里面去，后台有一个库存系统，负责获取消息，然后更新库存。
- 为什么使用消息队列？
  - 你的订单系统不发送消息到MQ，而是直接调用库存系统的一个接口，然后直接调用成功了，库存也更新了，那就不需要使用消息队列了呀
  - 使用消息队列的主要作用是：异步、解耦、削峰
- 消息队列都有什么优缺点？

- Kafka、activeMQ、RibbitMQ、RocketMQ都有什么优缺点？
- 如何保证消息队列的高可用？
- 如何保证消息不被重复消费？如何保证消息消费时的幂等性？
- 如何保证消息的可靠性传输，要是消息丢失了怎么办？
- 如何保证消息的顺序性？
- 如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？
- 如果让你写一个消息队列，该如何进行架构设计，说一下你的思路？

面试官问的问题不是发散的，而是从点、铺开，比如先聊一聊高并发的话题，就这个话题里面继续聊聊缓存、MQ等等东西。对于每个小话题，比如说MQ，就会从浅入深