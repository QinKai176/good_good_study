## Spring自定义starter

自定义starter的日志项目

### 1.新建maven项目

引入pom:

```xml
<groupId>com.demo</groupId>
<artifactId>logging-spring-boot-starter</artifactId>
<version>1.0-SNAPSHOT</version>

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.3</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-actuator-autoconfigure</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <!--可以让子项目选择是否依赖该包-->
        <optional>true</optional>
    </dependency>
</dependencies>
```

### 2.准备java类

TimeLog：

```
package com.betterarrow.logging;

public @interface TimeLog {
}
```

TimeLogProperties 

```
import org.springframework.boot.context.properties.ConfigurationProperties;

@Component
@ConfigurationProperties(prefix = "time.log")
public class TimeLogProperties {

    private Boolean enable;

    public boolean getEnable() {
        return enable;
    }

    public void setEnable(Boolean enable) {
        this.enable = enable;
    }
}
```

TimeAutoConfiguration

```
/**
 * @author qinkai
 */
@Configuration
@Aspect
@EnableAspectJAutoProxy
@ConditionalOnProperty(prefix = "time.log", name = "enable", havingValue = "true", matchIfMissing = true)
public class TimeAutoConfiguration {
    public static final Logger logger = LoggerFactory.getLogger(TimeAutoConfiguration.class);

    @Around("@annotation(com.betterarrow.logging.TimeLog)")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        //public java.lang.String org.javaboy.xxx.controller.hello()
        String name = proceedingJoinPoint.getSignature().toLongString().split(" ")[2];
        long startTime = System.currentTimeMillis();
        Object result = proceedingJoinPoint.proceed();
        long endTime = System.currentTimeMillis();
        logger.info("方法 {} 耗时 {} ms", name, endTime - startTime);
        return result;
    }
}
```

resource/META-INF:spring.factories

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.betterarrow.logging.TimeAutoConfiguration
```

### 3.第三方使用

```
time.log.enable=true
```

```
<dependency>
    <groupId>com.demo</groupId>
    <artifactId>logging-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

#### 4.扩展知识

```
@ConditionalOnProperty(prefix = "time.log", name = "enable", havingValue = "true", matchIfMissing = false)
```