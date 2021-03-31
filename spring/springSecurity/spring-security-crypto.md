## spring-security-crypto

### 1.包的引入

```
crypto

英 ['krɪptəʊ]
  秘密党员
```



### 2.pom引入

```
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-crypto</artifactId>
    <version>5.0.9.RELEASE</version>
</dependency>
```



### 2.类

获取用户信息：

org.springframework.security.core.userdetails.User;

org.springframework.security.core.userdetails.UserDetailsService;

校验账号密码：

org.springframework.security.authentication.dao.AbstractUserDetailsAuthenticationProvider



oauth2:

org.springframework.security.oauth2.provider.token.store.JwtAccessTokenConverter ;

org.springframework.security.oauth2.provider.client.BaseClientDetails;

org.springframework.security.oauth2.provider.ClientDetailsService;



异常：

org.springframework.security.oauth2.common.exceptions.OAuth2Exception;

org.springframework.security.web.authentication.AuthenticationFailureHandler;

com.winning.base.middleend.oauth2.handler.WinAuthenticationSuccessHandler;





### 3.注解

@interface EnableAuthorizationServer