## Spring面试题

```
1.简单介绍⼀下 Spring?有啥缺点?
优点：IOC  AOP 事务管理，一体化的框架集成
缺点：耦合性比较高，引入不常见的组件比较难

2. 为什么要有 SpringBoot?
便捷式的，集成tomcat的一键式开发部署；
减少配置；


3. 说出使⽤ Spring Boot 的主要优点
便捷式的，集成tomcat的一键式开发部署；
引入中间件方便快捷，教程多，文档多



4. 什么是 Spring Boot Starters?（不会）
Spring Boot Starters 是一系列依赖关系的集合，因为它的存在，项目的依赖之间的关系对我们来说变的更加简单了。


5. Spring Boot ⽀持哪些内嵌 Servlet 容器？
tomcat,jetty, netty

6. 如何在 Spring Boot 应⽤程序中使⽤ Jetty ⽽不是 Tomcat?
修改pom文件

7. 介绍⼀下@SpringBootApplication 注解  (忘了)
@SpringBootConfiguration  ---->说明是一个配置类，允许在上下文中注册额外的 bean 或导入其他配置类
@EnableAutoConfiguration：启用 SpringBoot 的自动配置机制
@ComponentScan  描被@Component (@Service,@Controller)注解的 bean，注解默认会扫描该类所在的包下所有的类



8. Spring Boot 的⾃动配置是如何实现的? 
@Configuration   
@EnableImport(ImportFrom.)



9. 开发 RESTful Web 服务常⽤的注解有哪些？
@Controller
@Bean
@Configuration
@Service
@Repository
@RestController
@Get
@Post


10. Spirng Boot 常⽤的两种配置⽂件
yml和property

11. 什么是 YAML？YAML 配置的优势在哪⾥ ?
可读写好

12. Spring Boot 常⽤的读取配置⽂  件的⽅法有哪些？
@Configuration


13. Spring Boot 加载配置⽂件的优先级了解么？
先读取yml，后读取property


14. 常⽤的 Bean 映射⼯具有哪些？
BeanUtils   dozer

15. Spring Boot 如何监控系统实际运⾏状况？
actuator

16. Spring Boot 如何做请求参数校验？
hibernate-core.jar


17. 如何使⽤ Spring Boot 实现全局异常处理？
aop?


18. Spring Boot 中如何实现定时任务 ?
分布式情况使用xxl-job



```

