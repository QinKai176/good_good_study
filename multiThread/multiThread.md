## 多线程

### 1.杀死线程的方式

window下：tasklist查看进程，taskkill杀死进程。

linux下，ps -ef查看进程，kill -9杀死进程。

java

```
jps：查看所有的java进程
jstack <pid>：查看进程所有的线程状态
jconsole:查看某个java进程的运行情况
```

### 2.创建线程的三种方式

Thread,Runnable,FutureTask

### 3.线程的私有区域-栈

每个线程启动后，虚拟机就会为其分配一块栈内存。

每个栈有多个栈帧组成，对应每次方法调用时占用的内存；

每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法。

### 4.线程上下文切换

因为一些原因导致cpu不再执行当前的线程，转而执行另一个线程的代码：

a.线程的cpu时间片用完

b.垃圾回收

c.有更高优先级的线程需要运行

d.线程自己调用了sleep,yield,wait,join,park,synchronized,lock等方法

### 5.线程的方法

![KPC5gY](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/KPC5gY.png)

### 6.线程的状态

NEW ,  

RUNNABLE,  

blocked, 

waiting, 

timed_waiting(sleep的时候),

 terminated

#### 6.1 五种状态

![image-20201213213843282](/Users/qinkai/Library/Application Support/typora-user-images/image-20201213213843282.png)

6.2 六种状态

![8gPjQt](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/8gPjQt.png)



![yN00Fq](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/yN00Fq.png)





### 7.线程的方法

#### 7.1 sleep

![cOSIba](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/cOSIba.png)

sleep可以防止cpu占用率达到100%

#### 7.2 yield

![DOUtaQ](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/DOUtaQ.png)

#### 7.3 join 

等待线程运行结束



















p35



