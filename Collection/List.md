##  List

### 1.ArrayList

#### 1.1 ArrayList和LinkedList

```
1.相同点
a.都是非线程安全的

2.不同点
a.ArrayList底层是Object数组，LinkedList底层是双向链表；
b.数组和双向链表区别：
b.1 arrayList查询时间复杂度为o(1),插入到最后时间复杂度为o(1),插入到指定位置时间复杂度为o（n）,删除的时间复杂度为o(n)
LinkedList查询时间复杂度为o(n),插入到最后时间复杂度为o(1),插入到指定位置时间复杂度为o(n),删除的时间复杂度为o(1)
b.2 对于for循环来说，arrayList使用for或者forEach方法，LinkedList使用iterator指针遍历；


```

#### 1.2 RandomAccess 

查看源码我们发现实际上 RandomAccess 接口中什么都没有定义。所以，在我看来 RandomAccess 接口不过是一个标识罢了。标识什么？ 标识实现这个接口的类具有随机访问功能。

ArraysList 实现了 RandomAccess 接口，就表明了他具有快速随机访问功能。 RandomAccess 接口只是标识，并不是说 ArraysList 实现 RandomAccess 接口才具有快速随机访问功能的！

#### 1.3 扩容机制（重要）

```
如果容量为空，第一次add的时候容量增长为10
int newCapacity = oldCapacity + (oldCapacity >> 1),所以 ArrayList 每次扩容之后容量都会变为原来的 1.5 倍左右（oldCapacity 为偶数就是 1.5 倍，否则是 1.5 倍左右），奇偶不同，比如 ：10+10/2 = 15, 33+33/2=49。如果是奇数的话会丢掉小数.
```

#### 1.4 线程非安全（重要）

#### 1.5 底层（重要）

数组

### 2.LinkedList

#### 2.1 底层

双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)

#### 2.2 遍历

不推荐用for循环遍历

### 附录

#### 1.参考

https://github.com/Snailclimb/JavaGuide/blob/master/docs/essential-content-for-interview/PreparingForInterview/%E7%BE%8E%E5%9B%A2%E9%9D%A2%E8%AF%95%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md#1-%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-mq-%E7%9A%84%E5%A5%97%E8%B7%AF