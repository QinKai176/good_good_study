## SONAR

### 1. Functional Interfaces should be as specialised as possible

参考文档：https://rules.sonarsource.com/java/RSPEC-4276

之前的代码：

```java
Function<String, String> testFunction = paramNo -> {    FindParamIdByParamNoCacheDO findParamIdByParamNoCacheDO = new FindParamIdByParamNoCacheDO(null, paramNo);    return cachePolicyManager.getKey(findParamIdByParamNoCacheDO);};
```

优化后的：

```java
UnaryOperator<String> testFunction = paramNo -> {    FindParamIdByParamNoCacheDO findParamIdByParamNoCacheDO = new FindParamIdByParamNoCacheDO(null, paramNo);    return cachePolicyManager.getKey(findParamIdByParamNoCacheDO);};
```