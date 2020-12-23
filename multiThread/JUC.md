##  JUC

### 1.LockSupport

#### 1.1 与wait/notify对比

a.unpark既可以在park()之前调用，也可以在park()之后调用；

wait和notify必须先wait后notify;

b.park和unpark以线程为单位来阻塞和唤醒线程；wait和notify只能随机唤醒一个等待线程；

c.wait和notify必须配合Object  Monitor一起使用，而park和unpark不必；

#### 1.2 原理

Park & unpark

每个线程都有自己的Parker对象，由三部分组成，_counter, _cond和 _mutex

Park:

![mrU7nf](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/mrU7nf.png)

unpark:

![PmFzgF](https://raw.githubusercontent.com/QinKai176/Image-Hosting/master/upic/PmFzgF.png)

### 2.ReentrantLock

#### 2.1 特点：

a.可中断；

b.可以设置为公平锁；

c.可以设置超时时间；

d.支持多个条件变量；

e.与synchronized一样，支持可重入；（可以再次获得这把锁）

#### 2.2 与synchronized的区别






