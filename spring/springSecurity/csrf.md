## CSRF

### 1.Spring-security-web-5.3.2.jar

#### 1.1 org.springframework.security.web.csrf



### 2.配置

CsrfTokenRepository接口

实现类CookieCsrfTokenRepository：信息存放在cookie

实现类HttpSessionCsrfTokenRepository:信息存放在session

实现类LazyCsrfTokenRepository: 必要的时候才创建token

实现类TestCsrfTokenRepository：测试时使用Mock





### 3.参考链接

https://www.freesion.com/article/4601556087/









