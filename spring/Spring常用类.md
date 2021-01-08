## Spring面试题

### 1.BeanNameAware

类似让用户知道自身的userId

### 2.ApplicationContextAware

实现这个接口，能够随时随地获取到applicationContext这个对象的实例；

### 3.ListableBeanFactory



### 4.CorsFilter

支持跨域请求

```
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.setAllowCredentials(true);
        corsConfiguration.addAllowedOrigin("*");
        corsConfiguration.addAllowedHeader("*");
        corsConfiguration.addAllowedMethod("*");
        UrlBasedCorsConfigurationSource urlBasedCorsConfigurationSource = new UrlBasedCorsConfigurationSource();
        urlBasedCorsConfigurationSource.registerCorsConfiguration("/**", corsConfiguration);
        return new CorsFilter(urlBasedCorsConfigurationSource);
    }
```

### 5.OncePerRequestFilter





### Todo

AnnotatedElementUtils.findMergedAnnotation作用