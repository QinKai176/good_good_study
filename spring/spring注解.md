## Spring注解

### 1.配置相关

@ConditionalOnProperty：

```
@ConditionalOnProperty(prefix = "time.log", name = "enable", havingValue = "true", matchIfMissing = false)
```

@Conditional:

![VCFFh6](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/VCFFh6.png)







### 2.扫描相关

@ComponentScan :basePackages默认扫描本文件下的所有类，也可以指定扫描的包

![GtgF9V](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/GtgF9V.png)

### 3.环境相关

@Profile



### 4.AOP

日志，事务，数据库操作

![ztph4Z](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/ztph4Z.png)

![OcBGiI](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/OcBGiI.png)

### 5.数据处理相关

responsebodyAdvice ：

对返回值进行处理返回；

RequestBodyAdvice :

对入参进行处理；