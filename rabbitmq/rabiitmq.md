##  Rabbitmq

### 1.安装使用

```
docker pull rabbitmq:3.7.7-management
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=my_vhost  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin df80af9ca0c9
```

### 2.rabbit的api

![l2UxEv](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/l2UxEv.png)