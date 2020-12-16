## java8的更新

### 1.Map的更新 

#### 1.1 新增merge接口

```java
efault V merge(K key, V value,
        BiFunction<? super V, ? super V, ? extends V> remappingFunction)
```



### 2.Function接口

#### 2.1 Function

```java
/**
 * Applies this function to the given argument.
 *
 * @param t the function argument
 * @return the function result
 */
R apply(T t);
```

#### 2.2 BiFunction

```
BiFunction
```

#### 2.3  @FunctionInterface

表示这个方法可以被lambda简化