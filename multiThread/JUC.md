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

a.可中断；通过设置lock.lockInterruptibly()来实现这个机制。

b.可以设置为公平锁；

c.可以设置超时时间；lock.tryLock(1,TimeUnit.second)

d.支持多个条件变量；condition，可以实现wait和notify类似的指定线程通知；

e.与synchronized一样，支持可重入；（可以再次获得这把锁）

#### 2.2 与synchronized的区别

### 3.CountDownLatch

门栓，可以控制线程执行的顺序，实现线程间的同步

### 4.CyclicBarrier

可以执行分段任务有序执行的场景。

### 5.Semaphore

递增的，用来限制能同时访问共享资源的线程上限；      

代码实例：

```java
Semaphore semaphore = new Semaphore(0);
List<Integer> list = new ArrayList<>();
Thread t1 = new Thread(() -> {
    try {
        for (int i = 0; i < 100000; i++) {
            list.add(1);
        }
        System.out.println("A:thread t1 end");
        semaphore.release();
    } catch (Exception e) {
        e.printStackTrace();
        System.out.println("B:thread t1 end");
        semaphore.release();
    }
});
Thread t2 = new Thread(() -> {
    try {
        for (int i = 0; i < 100000; i++) {
            list.add(1);
        }
        System.out.println("A:thread t2 end");
        semaphore.release();
    } catch (Exception e) {
        System.out.println("B:thread t2 end");
        e.printStackTrace();
        semaphore.release();
    }
});
t1.start();
t2.start();
System.out.println("here---");
semaphore.acquire(2);
System.out.println("预计list的size为：200000");
System.out.println("实际list的size为：" + list.size());
if (semaphore.getQueueLength() == 0) {
    System.out.println("main:here");
}
```

​                                            






