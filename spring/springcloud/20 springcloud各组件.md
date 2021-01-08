## Springcloud各组件

### 1.服务注册 Eureka

```
Eureka Server提供服务注册服务
各个节点启动后，会在EurekaServer中进行注册，这样Eurekaserver中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观地看到；

Eureka Client是一个java客户端，用于简化Eureka Server地交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳，默认周期是30s，如果Eureka Server在多个心跳周期内没有收到某个节点的心跳，EurekaServer将会从服务注册表中将这个服务节点移除(默认90s)
```

### 2.Eureka原理

```
三大角色：
Eureka Server提供服务注册和发现；
Service Provider服务提供方将自身服务注册到Eureka，从而使服务消费方能够找到；
Service Consumer服务消费方从Eureka获取注册服务列表，从而能够消费服务。
```

### 3.Eureka配置参数

```yaml
server: 
  port: 7001
 
eureka: 
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
#      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

### 4.Eureka保护机制

```
在自我保护中，Eureka Server会保护服务注册表中的信息，不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该Eureka Server就会自动退出自我保护模式。它的哲学是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话讲解：好死不如赖活
```

### 5.经典的CAP图

C:强一致性   A:高可用性   P:分布式容忍性

CA:传统ORACLE数据库

AP:大多数网站架构的选择

CP:Redis,Mongodb

分布式架构必须做出选择



### 6.负载均衡

```
LB,即负载均衡，在微服务或分布式集群中

集中式LB
在服务的消费方和提供方之间使用独立的LB设施（可以是硬件，比如F5，也可以是软件，比如nginx），由该设施负责将访问请求通过某种策略转发到服务的提供方；、

进程内LB
将LB逻辑集成到消费方，消费方从服务注册中心获知哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取服务提供方的地址。
```



### 7.Ribbon的工作机理

```
ribbon
RoundRibinRule  轮询（默认）
RandonRule  随机
```



### 8.Feign

```
Feign是一个声明式的webservice客户端，使用feign能让编写web service客户端更加简单，它的使用方法是定义一个接口，然后在上面添加注解，同时支持JAX-RS标准的注解。Feign也支持可拔插式的编码器和解码器。Spring Cloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用，以便支持负载均衡。
```





Feign旨在使编写java http客户端更加容易；

前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一处接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它（以前是Dao接口上面标注Mapper注解），现在是一个微服务接口上面标注一个Feign注解即可），即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon时，自动封装服务调用客户端的开发量。