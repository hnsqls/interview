## AQS是什么

全称是 AbstractQueuedSynchronizer，即抽象队列同步器，它是构建锁或者其他同步组件的基础框架（工具）。比如ReentrantLock,Semaphore,CountDownLatch。

> 工作原理

锁的状态字段 `private volatile int state`  0表示无锁，1表示有锁。volatile 保证了多线程下state可见性。

在多线程的获取锁的情况下，通过CAS操作更改锁的字段state 0 -> 1,哪个线程成功修改了锁的state字段，就获得了锁的资源。其他线程在请求获取该锁，判断锁对象的state = 1，就无法获取锁。就在FIFO双向的等待队列里，当获得锁的线程释放锁时，同时唤醒FIFO的队列头元素，如果此时又来了新线程，那么新线程会和刚唤醒的线程争夺，通过cas操作确保获得锁的原子性。.



> 核心原理

AQS维护了一个 volatile state字段表示是否获被线程获取，和一个FIFO的双向队列来维护等待队列。并且是非公平锁。

