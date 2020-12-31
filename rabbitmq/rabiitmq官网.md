#  Rabbitmq

## 1.introduction

### 1.1 About rabbitmq

> RabbitMQ is a message broker: it accepts and forwards messages. You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that Mr. or Ms. Mailperson will eventually deliver the mail to your recipient. In this analogy, RabbitMQ is a post box, a post office and a postman

### 1.2 jargon

#### 1.2.1 producing

*Producing* means nothing more than sending. A program that sends messages is a *producer*.

#### 1.2.2 queue

*A queue* is the name for a post box which lives inside RabbitMQ.

Many *producers* can send messages that go to one queue, and many *consumers* can try to receive data from one *queue*. This is how we represent a queue:

#### 1.2.3 Consuming

*Consuming* has a similar meaning to receiving. A *consumer* is a program that mostly waits to receive messages.

#### 1.2.4 Exchange

> The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Actually, quite often the producer doesn't even know if a message will be delivered to any queue at all.

Instead, the producer can only send messages to an *exchange*. An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives. Should it be appended to a particular queue? Should it be appended to many queues? Or should it get discarded. The rules for that are defined by the *exchange type*.

![5yVXfN](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/5yVXfN.png)

There are a few exchange types available: direct, topic, headers and fanout. 

#### 1.2.5 Channel

> 在`AMQP`协议中，有`channel`的概念，在`RabbitMq`中,`channel`表示逻辑连接或者叫虚拟连接,是棣属于`TCP`连接的。一个`TCP`连接里可以创建多个`channel`,在`Rabbit MQ`里，消息的发送和接收都是基于`channel`的。



## 2.Hello World

点对点的模式

## 3.Work Queues

（Fanout）

工作模式 广播

Default: Round-robin

### 3.1 Message acknowledgment

> Doing a task can take a few seconds. You may wonder what happens if one of the consumers starts a long task and dies with it only partly done. With our current code, once RabbitMQ delivers a message to the consumer it immediately marks it for deletion. In this case, if you kill a worker we will lose the message it was just processing. We'll also lose all the messages that were dispatched to this particular worker but were not yet handled.

### 3.2 Message durability

At this point we're sure that the task_queue queue won't be lost even if RabbitMQ restarts;

 Note on message persistence:

> Marking messages as persistent doesn't fully guarantee that a message won't be lost. Although it tells RabbitMQ to save the message to disk, there is still a short time window when RabbitMQ has accepted a message and hasn't saved it yet. Also, RabbitMQ doesn't do fsync(2) for every message -- it may be just saved to cache and not really written to the disk. The persistence guarantees aren't strong, but it's more than enough for our simple task queue. If you need a stronger guarantee then you can use [publisher confirms](https://www.rabbitmq.com/confirms.html).
>

### 3.3 Fair dispatch

## 4.Routing

### 4.1 Direct exchange

![QejsoI](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/QejsoI.png)





## 5.Publish/Subscribe

> In the [previous tutorial](https://www.rabbitmq.com/tutorials/tutorial-two-java.html) we created a work queue. The assumption behind a work queue is that each task is delivered to exactly one worker. In this part we'll do something completely different -- we'll deliver a message to multiple consumers. This pattern is known as "publish/subscribe".



## 6.topic 

> In the [previous tutorial](https://www.rabbitmq.com/tutorials/tutorial-four-java.html) we improved our logging system. Instead of using a fanout exchange only capable of dummy broadcasting, we used a direct one, and gained a possibility of selectively receiving the logs. Although using the direct exchange improved our system, it still has limitations - it can't do routing based on multiple criteria





## Appendix

### 1.Question

### 2.Reference

a.Spring RabbitMQ Channel理解

https://blog.csdn.net/linsongbin1/article/details/100892678

b.rabbitmq官方文档

https://www.rabbitmq.com/tutorials/tutorial-four-java.html





