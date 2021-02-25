## Ribbon

### 1.饥饿加载

现象：服务都成功启动的时候第一次访问会有报错的情况发生,但是之后又恢复正常访问；

原因：主要是Ribbon进行客户端负载均衡的Client并不是在服务启动的时候就初始化好的，而是在调用的时候才会去创建相应的Client，所以第一次调用的耗时不仅仅包含发送HTTP请求的时间，还包含了创建RibbonClient的时间，这样一来如果创建时间速度较慢，同时设置的超时时间又比较短的话，很容易就会出现上面所描述的显现。

```
ribbon.eager-load.enabled=true

ribbon.eager-load.clients=cloud-shop-userservice

参数说明:

ribbon.eager-load.enabled : 开启Ribbon的饥饿加载模式

ribbon.eager-load.clients: 指定需要饥饿加载的服务名
```

