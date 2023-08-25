# 2021Study-JUC

> https://github.com/Ezrabin/juc_atguigu/tree/main
>
> 在github搜索juc_atguigu就能找到



## 1. 什么是JUC

![1635043688257](images/1635043688257.png)

## 2. 多线程回顾

> 进程与线程

**进程**：一个程序

一个进程往往包含多个线程，至少包含一个！

java默认有几个线程？2个，main、GC

**进程是资源分配的最小单位**

**线程**：开了一个进程Typora，写字、自动保存

对于Java而言：Thread、Runable、Callable

**线程是程序执行的最小单位**

Java可以开启线程吗？开不了，参见源码`private native void start0();`

---

> 并行与并发

**并发**（多线程操作一个资源）

**并行**（多个线程同时执行）

并发编程的本质：想充分利用CPU的资源

---

> 多线程的状态有6个，Thread.State

```java
public enum State {
        /**
         * Thread state for a thread which has not yet started.
         新生
         */
        NEW,

        /**
         * Thread state for a runnable thread.  A thread in the runnable
         * state is executing in the Java virtual machine but it may
         * be waiting for other resources from the operating system
         * such as processor.
         运行
         */
        RUNNABLE,

        /**
         * Thread state for a thread blocked waiting for a monitor lock.
         * A thread in the blocked state is waiting for a monitor lock
         * to enter a synchronized block/method or
         * reenter a synchronized block/method after calling
         * {@link Object#wait() Object.wait}.
         阻塞
         */
        BLOCKED,

        /**
         * Thread state for a waiting thread.
         * A thread is in the waiting state due to calling one of the
         * following methods:
         * <ul>
         *   <li>{@link Object#wait() Object.wait} with no timeout</li>
         *   <li>{@link #join() Thread.join} with no timeout</li>
         *   <li>{@link LockSupport#park() LockSupport.park}</li>
         * </ul>
         *
         * <p>A thread in the waiting state is waiting for another thread to
         * perform a particular action.
         *
         * For example, a thread that has called <tt>Object.wait()</tt>
         * on an object is waiting for another thread to call
         * <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on
         * that object. A thread that has called <tt>Thread.join()</tt>
         * is waiting for a specified thread to terminate.
         等待
         */
        WAITING,(不见不散)

        /**
         * Thread state for a waiting thread with a specified waiting time.
         * A thread is in the timed waiting state due to calling one of
         * the following methods with a specified positive waiting time:
         * <ul>
         *   <li>{@link #sleep Thread.sleep}</li>
         *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
         *   <li>{@link #join(long) Thread.join} with timeout</li>
         *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
         *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
         * </ul>
         超时等待
         */
        TIMED_WAITING,(过时不候)

        /**
         * Thread state for a terminated thread.
         * The thread has completed execution.
         终止
         */
        TERMINATED;(终结)
    }
```

----

> wait和sleep的区别

1. wait=>Object，sleep=>Thread
2. wait会释放锁；sleep抱着锁睡觉，不会释放
3. wait必须在同步代码块中，sleep可以在任何地方睡
4. wait不需要捕获异常，sleep要捕获异常
5. 他们都能被interrupt()方法中断
6. 他们在哪里睡就在哪里醒

> 管程Monitor对象，java中叫锁，操作系统中叫监视器

是一种同步机制，保证同一时间，只有一个线程访问被保护数据或者代码

> 用户线程和守护线程

用户线程：自定义线程。主线程结束了，用户线程还在运行，则jvm存活

守护线程：GC线程。没有用户线程了，都是守护线程，则jvm结束

![用户线程和守护线程](./images/image-20221224174959156.png)





## 3. Lock锁

---

> interface Lock

![1635047188588](images/1635047188588.png)

公平锁：十分公平，可以先来后到；效率相对低

非公平锁：十分不公平，可以插队（Java默认）；线程可能饿死，效率高

---

> synchronized 和 lock区别

1. synchronized是内置Java关键字，Lock是一个java接口
2. synchronized无法判断获取锁的状态，Lock可以判断是否获取到了锁。`tryLock()`
3. synchronized会自动释放锁，Lock必须手动释放锁！如果不释放，**死锁**，因此适用Lock需要在finally块中释放锁`unlock()`
4. synchronized 线程1（获得锁）线程2（傻傻的等），不能响应中断；Lock锁不一定会等待下去，可以让等待锁的线程响应中断`lockInterruptibly()`
5. synchronized可重入锁，不可以响应中断，非公平；Lock，可重入锁，可以当锁空闲时获取锁（`lock.tryLock()`）,非公平（可以设置）
6. synchronized适合锁少量的代码同步问题，Lock适合锁大量的同步代码（性能上，当竞争资源非常激烈时，Lock的性能要远远优于synchronized）
7. synchronized没有Condition，Lock可以有Condition`newCondition()`

### 虚假唤醒

> 解决方法:将if换为while
>
> wait()需要在循环中使用，否则会出现跳过if导致虚假唤醒。
>
> wait从哪里等待就从哪里唤醒

#### synchronized解决虚假唤醒

```java
package com.atguigu.sync;

//第一步 创建资源类，定义属性和操作方法
class Share {
    //初始值
    private int number = 0;
    //+1的方法
    public synchronized void incr() throws InterruptedException {
        //第二步 判断 干活 通知
        while(number != 0) { //判断number值是否是0，如果不是0，等待
            this.wait(); //在哪里睡，就在哪里醒
        }
        //如果number值是0，就+1操作
        number++;
        System.out.println(Thread.currentThread().getName()+" :: "+number);
        //通知其他线程
        this.notifyAll();
    }

    //-1的方法
    public synchronized void decr() throws InterruptedException {
        //判断
        while(number != 1) {
            this.wait();
        }
        //干活
        number--;
        System.out.println(Thread.currentThread().getName()+" :: "+number);
        //通知其他线程
        this.notifyAll();
    }
}

public class ThreadDemo1 {
    //第三步 创建多个线程，调用资源类的操作方法
    public static void main(String[] args) {
        Share share = new Share();
        //创建线程
        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.incr(); //+1
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"AA").start();

        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.decr(); //-1
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"BB").start();

        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.incr(); //+1
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"CC").start();

        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.decr(); //-1
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"DD").start();
    }
}
```



#### lock锁下解决虚假唤醒

```java
package com.atguigu.lock;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

//第一步 创建资源类，定义属性和操作方法
class Share {
    private int number = 0;

    //创建Lock
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    //+1
    public void incr() throws InterruptedException {
        //上锁
        lock.lock();
        try {
            //判断
            while (number != 0) {
                condition.await();
            }
            //干活
            number++;
            System.out.println(Thread.currentThread().getName()+" :: "+number);
            //通知
            condition.signalAll();
        }finally {
            //解锁
            lock.unlock();
        }
    }

    //-1
    public void decr() throws InterruptedException {
        lock.lock();
        try {
            while(number != 1) {
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName()+" :: "+number);
            condition.signalAll();
        }finally {
            lock.unlock();
        }
    }
}

public class ThreadDemo2 {

    public static void main(String[] args) {
        Share share = new Share();
        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.incr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"AA").start();
        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.decr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"BB").start();

        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.incr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"CC").start();
        new Thread(()->{
            for (int i = 1; i <=10; i++) {
                try {
                    share.decr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"DD").start();
    }

}
```



> 可重入锁 （又称递归锁）

进入第一道大门之后，房间内的门就不用重新打开了

### 死锁

什么是死锁：

两个或两个以上进程在执行过程中，因为争夺资源而造成的一种相互等待的现象，如果没有外力干涉，将无法执行下去

产生死锁的原因：

1. 系统资源不足
2. 进程运行推进顺序不合适
3. 资源分配不当

> 验证是否死锁

1. `jps -l`查看运行中的java进程
2. `jstack 进程号`通过jvm自带堆栈跟踪工具查看是否死锁

```java
package com.atguigu.sync;

import java.util.concurrent.TimeUnit;

/**
 * 演示死锁
 */
public class DeadLock {

    //创建两个对象
    static Object a = new Object();
    static Object b = new Object();

    public static void main(String[] args) {
        new Thread(()->{
            synchronized (a) {
                System.out.println(Thread.currentThread().getName()+" 持有锁a，试图获取锁b");
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                synchronized (b) {
                    System.out.println(Thread.currentThread().getName()+" 获取锁b");
                }
            }
        },"A").start();

        new Thread(()->{
            synchronized (b) {
                System.out.println(Thread.currentThread().getName()+" 持有锁b，试图获取锁a");
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                synchronized (a) {
                    System.out.println(Thread.currentThread().getName()+" 获取锁a");
                }
            }
        },"B").start();
    }
}
```



## 4. 线程间定制化通信

```java
public class C {
    public static void main(String[] args) {
        Data3 data3=new Data3();

        new Thread(()->{
            for(int i=0;i<10;i++){
                try {
                    data3.printA ();
                } catch (InterruptedException e) {
                    e.printStackTrace ();
                }
            }
        },"A").start ();
        new Thread(()->{
            for(int i=0;i<10;i++){
                try {
                    data3.printB();
                } catch (InterruptedException e) {
                    e.printStackTrace ();
                }
            }
        },"B").start ();
        new Thread(()->{
            for(int i=0;i<10;i++){
                try {
                    data3.printC ();
                } catch (InterruptedException e) {
                    e.printStackTrace ();
                }
            }
        },"C").start ();

    }
}

// 判断等待，业务，通知
class Data3{
    private int num=0;
    Lock lock=new ReentrantLock (  );
    Condition condition1 = lock.newCondition ();
    Condition condition2 = lock.newCondition ();
    Condition condition3 = lock.newCondition ();
    private int number=1;//1A,2B,3C
    public  void printA() throws InterruptedException {
        lock.lock ();
        try {
            while(number!=1){
                condition1.await ();
            }
            System.out.println (Thread.currentThread ().getName ()+"=>A");
            number=2;
            condition2.signal ();
        } catch (Exception e) {
            e.printStackTrace ();
        } finally {
            lock.unlock ();
        }

    }
    public  void printB() throws InterruptedException {
        lock.lock ();
        try {
            while(number!=2){
                condition2.await ();
            }
            System.out.println (Thread.currentThread ().getName ()+"=>B");
            number=3;
            condition3.signal ();
        } catch (Exception e) {
            e.printStackTrace ();
        } finally {
            lock.unlock ();
        }

    }
    public  void printC() throws InterruptedException {
        lock.lock ();
        try {
            while(number!=3){
                condition3.await ();
            }
            System.out.println (Thread.currentThread ().getName ()+"=>C");
            number=1;
            condition1.signal ();
        } catch (Exception e) {
            e.printStackTrace ();
        } finally {
            lock.unlock ();
        }

    }

```



## 5. 锁对象

> 普通synchronized锁的是调用者对象；
>
> 静态synchronized锁的是Class 类；
>
> 同步代码块锁的是synchronized括号内的对象

```java
package com.atguigu.sync;

import java.util.concurrent.TimeUnit;

class Phone {

    public static synchronized void sendSMS() throws Exception {
        //停留4秒
        TimeUnit.SECONDS.sleep(4);

        System.out.println("------sendSMS");
    }

    public synchronized void sendEmail() throws Exception {
        System.out.println("------sendEmail");
    }

    public void getHello() {
        System.out.println("------getHello");
    }
}

/**
 * @Description: 8锁
 *
1 标准访问，先打印短信还是邮件
------sendSMS
------sendEmail

2 停4秒在短信方法内，先打印短信还是邮件
------sendSMS
------sendEmail

3 新增普通的hello方法，是先打短信还是hello
------getHello
------sendSMS

4 现在有两部手机，先打印短信还是邮件
------sendEmail
------sendSMS

5 两个静态同步方法，1部手机，先打印短信还是邮件
------sendSMS
------sendEmail

6 两个静态同步方法，2部手机，先打印短信还是邮件
------sendSMS
------sendEmail

7 1个静态同步方法,1个普通同步方法，1部手机，先打印短信还是邮件
------sendEmail
------sendSMS

8 1个静态同步方法,1个普通同步方法，2部手机，先打印短信还是邮件
------sendEmail
------sendSMS

 */

public class Lock_8 {
    public static void main(String[] args) throws Exception {

        Phone phone = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            try {
                phone.sendSMS();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }, "AA").start();

        Thread.sleep(100);

        new Thread(() -> {
            try {
                phone.sendEmail();
               // phone.getHello();
                //phone2.sendEmail();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }, "BB").start();
    }
}
```



## 6. 线程安全集合类

```java
package com.atguigu.lock;

import java.util.*;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.CopyOnWriteArraySet;

/**
 * list集合线程不安全
 */
public class ThreadDemo4 {
    public static void main(String[] args) {
        //创建ArrayList集合
//        List<String> list = new ArrayList<>();
        // Vector解决
//        List<String> list = new Vector<>();

        //Collections解决
//        List<String> list = Collections.synchronizedList(new ArrayList<>());

        // CopyOnWriteArrayList解决
//        List<String> list = new CopyOnWriteArrayList<>();
//        for (int i = 0; i <30; i++) {
//            new Thread(()->{
//                //向集合添加内容
//                list.add(UUID.randomUUID().toString().substring(0,8));
//                //从集合获取内容
//                System.out.println(list);
//            },String.valueOf(i)).start();
//        }

        //演示Hashset
//        Set<String> set = new HashSet<>();

//        Set<String> set = new CopyOnWriteArraySet<>();
//        for (int i = 0; i <30; i++) {
//            new Thread(()->{
//                //向集合添加内容
//                set.add(UUID.randomUUID().toString().substring(0,8));
//                //从集合获取内容
//                System.out.println(set);
//            },String.valueOf(i)).start();
//        }

        //演示HashMap
//        Map<String,String> map = new HashMap<>();

        Map<String,String> map = new ConcurrentHashMap<>();
        for (int i = 0; i <30; i++) {
            String key = String.valueOf(i);
            new Thread(()->{
                //向集合添加内容
                map.put(key,UUID.randomUUID().toString().substring(0,8));
                //从集合获取内容
                System.out.println(map);
            },String.valueOf(i)).start();
        }
    }
}
```



## 7. Callable

> 1. 可以有返回值
> 2. 可以抛出异常
> 3. 方法不同，call()
>
> 细节：
>
> 1. 有缓存
> 2. 结果可能需要等待，会阻塞  
>
> ---
>
> 找一个类，既和Runnable有关系，也和Callable有关系
>
> - Runnable接口有子接口RunnableFuture,RunnableFuture有实现类FutureTask
> - FutureTask构造可以传递Callable

```java
package com.atguigu.callable;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

//比较两个接口
//实现Runnable接口
class MyThread1 implements Runnable {
    @Override
    public void run() {

    }
}

//实现Callable接口
class MyThread2 implements Callable {

    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName()+" come in callable");
        return 200;
    }
}

public class Demo1 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //Runnable接口创建线程
        new Thread(new MyThread1(),"AA").start();

        //Callable接口,报错
       // new Thread(new MyThread2(),"BB").start();

        //FutureTask
        FutureTask<Integer> futureTask1 = new FutureTask<>(new MyThread2());

        //lam表达式
        FutureTask<Integer> futureTask2 = new FutureTask<>(()->{
            System.out.println(Thread.currentThread().getName()+" come in callable");
            return 1024;
        });

        //创建一个线程
        new Thread(futureTask2,"lucy").start();
        new Thread(futureTask1,"mary").start();

//        while(!futureTask2.isDone()) {
//            System.out.println("wait.....");
//        }
        //调用FutureTask的get方法
        System.out.println(futureTask2.get());

        System.out.println(futureTask1.get());

        System.out.println(Thread.currentThread().getName()+" come over");
        //FutureTask原理  未来任务
        /**
         * 1、老师上课，口渴了，去买票不合适，讲课线程继续。
         *   单开启线程找班上班长帮我买水，把水买回来，需要时候直接get
         *
         * 2、4个同学， 1同学 1+2...5   ，  2同学 10+11+12....50， 3同学 60+61+62，  4同学 100+200
         *      第2个同学计算量比较大，
         *     FutureTask单开启线程给2同学计算，先汇总 1 3 4 ，最后等2同学计算位完成，统一汇总
         *
         * 3、考试，做会做的题目，最后看不会做的题目
         *
         * 汇总一次
         *
         */

    }
}
```



## 8. JUC常用辅助类

### 8.1 CountDownLatch

减法计数器

![1635132174680](images/1635132174680.png)

await():等待计数器归零后执行

countDown():计数器数量-1

```java
package com.atguigu.juc;

import java.util.concurrent.CountDownLatch;

//演示 CountDownLatch
public class CountDownLatchDemo {
    //6个同学陆续离开教室之后，班长锁门
    public static void main(String[] args) throws InterruptedException {

        //创建CountDownLatch对象，设置初始值
        CountDownLatch countDownLatch = new CountDownLatch(6);

        //6个同学陆续离开教室之后
        for (int i = 1; i <=6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" 号同学离开了教室");

                //计数  -1
                countDownLatch.countDown();

            },String.valueOf(i)).start();
        }

        //等待
        countDownLatch.await();

        System.out.println(Thread.currentThread().getName()+" 班长锁门走人了");
    }
}
```



### 8.2 CyclicBarrier

加法计数器

`new CyclicBarrier(7,()->{})`:集齐7龙珠就执行()->{}

![1635132415426](images/1635132415426.png)

```java
package com.atguigu.juc;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

//集齐7颗龙珠就可以召唤神龙
public class CyclicBarrierDemo {

    //创建固定值
    private static final int NUMBER = 7;

    public static void main(String[] args) {
        //创建CyclicBarrier
        CyclicBarrier cyclicBarrier =
                new CyclicBarrier(NUMBER,()->{
                    System.out.println("*****集齐7颗龙珠就可以召唤神龙");
                });

        //集齐七颗龙珠过程
        for (int i = 1; i <=7; i++) {
            new Thread(()->{
                try {
                    System.out.println(Thread.currentThread().getName()+" 星龙被收集到了");
                    //等待
                    cyclicBarrier.await();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            },String.valueOf(i)).start();
        }
    }
}
```




### 8.3 Semaphore

信号量

`new Semaphore(3)`:资源为3

acquire():假设如果已经满了，等待被释放为止

release():释放1个资源，唤醒等待的线程

作用：多个共享资源互斥使用，并发限流，控制最大的线程数

```java
package com.atguigu.juc;

import java.util.Random;
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

//6辆汽车，停3个车位
public class SemaphoreDemo {
    public static void main(String[] args) {
        //创建Semaphore，设置许可数量
        Semaphore semaphore = new Semaphore(3);

        //模拟6辆汽车
        for (int i = 1; i <=6; i++) {
            new Thread(()->{
                try {
                    //抢占
                    semaphore.acquire();

                    System.out.println(Thread.currentThread().getName()+" 抢到了车位");

                    //设置随机停车时间
                    TimeUnit.SECONDS.sleep(new Random().nextInt(5));

                    System.out.println(Thread.currentThread().getName()+" ------离开了车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    //释放
                    semaphore.release();
                }
            },String.valueOf(i)).start();
        }
    }
}
```



## 9. 读写锁ReadWriteLock

> 读写锁特点:
>
> 读-读 可以共存
>
> 读-写 不可共存
>
> 写-写 不可共存
>
> 读写锁的缺点：
>
> 1. 会造成锁饥饿，一直读，没有写
> 2. 读时候不能写，只有读完成了才可以写，而写的时候可以读

```java
package com.atguigu.readwrite;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

//资源类
class MyCache {
    //创建map集合
    private volatile Map<String,Object> map = new HashMap<>();

    //创建读写锁对象
    private ReadWriteLock rwLock = new ReentrantReadWriteLock();

    //放数据
    public void put(String key,Object value) {
        //添加写锁
        rwLock.writeLock().lock();

        try {
            System.out.println(Thread.currentThread().getName()+" 正在写操作"+key);
            //暂停一会
            TimeUnit.MICROSECONDS.sleep(300);
            //放数据
            map.put(key,value);
            System.out.println(Thread.currentThread().getName()+" 写完了"+key);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            //释放写锁
            rwLock.writeLock().unlock();
        }
    }

    //取数据
    public Object get(String key) {
        //添加读锁
        rwLock.readLock().lock();
        Object result = null;
        try {
            System.out.println(Thread.currentThread().getName()+" 正在读取操作"+key);
            //暂停一会
            TimeUnit.MICROSECONDS.sleep(300);
            result = map.get(key);
            System.out.println(Thread.currentThread().getName()+" 取完了"+key);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            //释放读锁
            rwLock.readLock().unlock();
        }
        return result;
    }
}

public class ReadWriteLockDemo {
    public static void main(String[] args) throws InterruptedException {
        MyCache myCache = new MyCache();
        //创建线程放数据
        for (int i = 1; i <=5; i++) {
            final int num = i;
            new Thread(()->{
                myCache.put(num+"",num+"");
            },String.valueOf(i)).start();
        }

        TimeUnit.MICROSECONDS.sleep(300);

        //创建线程取数据
        for (int i = 1; i <=5; i++) {
            final int num = i;
            new Thread(()->{
                myCache.get(num+"");
            },String.valueOf(i)).start();
        }
    }
}
```

### 写锁降级为读锁

```java
package com.atguigu.readwrite;

import java.util.concurrent.locks.ReentrantReadWriteLock;

//演示读写锁降级
public class Demo1 {

    public static void main(String[] args) {
        //可重入读写锁对象
        ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
        ReentrantReadWriteLock.ReadLock readLock = rwLock.readLock();//读锁
        ReentrantReadWriteLock.WriteLock writeLock = rwLock.writeLock();//写锁

        //锁降级
        //2 获取读锁
        readLock.lock();
        System.out.println("---read");

        //1 获取写锁
        writeLock.lock();
        System.out.println("atguigu");

        //3 释放写锁
        //writeLock.unlock();

        //4 释放读锁
        //readLock.unlock();
    }
}
```



## 10. 阻塞队列

![阻塞队列API查看](./images/image-20230727144211371.png)



| 方式       | 抛出异常  | 有返回值 | 阻塞等待 | 超时等待  |
| ---------- | --------- | -------- | -------- | --------- |
| 添加       | add()     | offer()  | put()    | offer(,,) |
| 移除       | remove()  | poll()   | take()   | poll(,,)  |
| 检测队列首 | element() | peek()   | 不可用   | 不可用    |

```java
package com.atguigu.queue;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

//阻塞队列
public class BlockingQueueDemo {

    public static void main(String[] args) throws InterruptedException {
        //创建阻塞队列
        BlockingQueue<String> blockingQueue = new ArrayBlockingQueue<>(3);

        //第一组
//        System.out.println(blockingQueue.add("a"));
////        System.out.println(blockingQueue.add("b"));
////        System.out.println(blockingQueue.add("c"));
////        //System.out.println(blockingQueue.element());
////
////        //System.out.println(blockingQueue.add("w"));
////        System.out.println(blockingQueue.remove());
////        System.out.println(blockingQueue.remove());
////        System.out.println(blockingQueue.remove());
////        System.out.println(blockingQueue.remove());

        //第二组
//        System.out.println(blockingQueue.offer("a"));
//        System.out.println(blockingQueue.offer("b"));
//        System.out.println(blockingQueue.offer("c"));
//        System.out.println(blockingQueue.offer("www"));
//
//        System.out.println(blockingQueue.poll());
//        System.out.println(blockingQueue.poll());
//        System.out.println(blockingQueue.poll());
//        System.out.println(blockingQueue.poll());

        //第三组
//        blockingQueue.put("a");
//        blockingQueue.put("b");
//        blockingQueue.put("c");
//        //blockingQueue.put("w");
//
//        System.out.println(blockingQueue.take());
//        System.out.println(blockingQueue.take());
//        System.out.println(blockingQueue.take());
//        System.out.println(blockingQueue.take());

        //第四组
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        System.out.println(blockingQueue.offer("w",3L, TimeUnit.SECONDS));
    }
}
```



## 11. 线程池（重点）

![Executors](./images/image-20230727144941705.png)

![Executor](./images/image-20230727145104095.png)

![阿里线程池规范](images/1635162492969.png)

```java
package com.atguigu.pool;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//演示线程池三种常用分类
public class ThreadPoolDemo1 {
    public static void main(String[] args) {
        //一池五线程
        ExecutorService threadPool1 = Executors.newFixedThreadPool(5); //5个窗口

        //一池一线程
        ExecutorService threadPool2 = Executors.newSingleThreadExecutor(); //一个窗口

        //一池可扩容线程
        ExecutorService threadPool3 = Executors.newCachedThreadPool();
        //10个顾客请求
        try {
            for (int i = 1; i <=10; i++) {
                //执行
                threadPool3.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" 办理业务");
                });
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭
            threadPool3.shutdown();
        }

    }

}
```

```java
package com.atguigu.pool;

import java.util.concurrent.*;

//自定义线程池创建
public class ThreadPoolDemo2 {
    public static void main(String[] args) {
        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                2L,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );

        //10个顾客请求
        try {
            for (int i = 1; i <=10; i++) {
                //执行
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" 办理业务");
                });
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭
            threadPool.shutdown();
        }
    }
```



![1635163264670](images/1635163264670.png)

![1635163505834](images/1635163505834.png)

![1635163707645](images/1635163707645.png)

![线程池底层工作流程](./images/image-20221226163512441.png)

> 四种拒绝策略

![1635163893985](images/1635163893985.png)

```
ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。 
ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。 
ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列中等待最久的任务，然后重新尝试执行任务（重复此过程）
ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务 
```

 ThreadPoolExecutor 的几个重要方法

- execute()方法实际上是Executor中声明的方法，在ThreadPoolExecutor进行了具体的实现，这个方法是ThreadPoolExecutor的核心方法，通过这个方法可以向线程池提交一个任务，交由线程池去执行。
- submit()方法是在ExecutorService中声明的方法，在AbstractExecutorService就已经有了具体的实现，在ThreadPoolExecutor中并没有对其进行重写，这个方法也是用来向线程池提交任务的，但是它和execute()方法不同，它能够返回任务执行的结果，去看submit()方法的实现，会发现它实际上还是调用的execute()方法，只不过它利用了Future来获取任务执行结果（Future相关内容将在下一篇讲述）。
-  shutdown()和shutdownNow()是用来关闭线程池的。 

---

最大线程池应该如何定义：

1. CPU密集型`Runtime.getRuntime().avaiableProcessors()`
2. IO密集型 大于你程序中十分耗IO的线程个数



## 14. ForkJoin

[CSDN-Fork/Join框架基本使用](https://blog.csdn.net/tyrroo/article/details/81390202)

[CSDN-Stream.iterate 和 LongStream.rangeClosed 并行处理](https://blog.csdn.net/CmdSmith/article/details/84139659)

> 什么是ForkJoin

ForkJoin在JDK1.7并行执行任务！提高效率，大数据量

大数据：Map Reduce(把大任务拆成小任务)

> ForkJoin特点：工作窃取

这里面维护的都是双端队列

![1635233174696](images/1635233174696.png)

 ```JAVA
package com.atguigu.forkjoin;

import java.util.concurrent.*;

class MyTask extends RecursiveTask<Integer> {

    //拆分差值不能超过10，计算10以内运算
    private static final Integer VALUE = 10;
    private int begin ;//拆分开始值
    private int end;//拆分结束值
    private int result ; //返回结果

    //创建有参数构造
    public MyTask(int begin,int end) {
        this.begin = begin;
        this.end = end;
    }

    //拆分和合并过程
    @Override
    protected Integer compute() {
        //判断相加两个数值是否大于10
        if((end-begin)<=VALUE) {
            //相加操作
            for (int i = begin; i <=end; i++) {
                result = result+i;
            }
        } else {//进一步拆分
            //获取中间值
            int middle = (begin+end)/2;
            //拆分左边
            MyTask task01 = new MyTask(begin,middle);
            //拆分右边
            MyTask task02 = new MyTask(middle+1,end);
            //调用方法拆分
            task01.fork();
            task02.fork();
            //合并结果
            result = task01.join()+task02.join();
        }
        return result;
    }
}

public class ForkJoinDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //创建MyTask对象
        MyTask myTask = new MyTask(0,100);
        //创建分支合并池对象
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Integer> forkJoinTask = forkJoinPool.submit(myTask);
        //获取最终合并之后结果
        Integer result = forkJoinTask.get();
        System.out.println(result);
        //关闭池对象
        forkJoinPool.shutdown();
    }
}
 ```

```java
public class StreamTest {

    public static void main(String[] args) {
        int i = Runtime.getRuntime().availableProcessors();

        // for
        System.out.println(measureSumPerf(StreamTest::iterativeSum, 10_000_000) + " msecs");
        // Stream.iterate
        System.out.println(measureSumPerf(StreamTest::sequentialSum, 10_000_000) + " msecs");
        // parallel Stream.iterate
        System.out.println(measureSumPerf(StreamTest::parallelSum, 10_000_000) + " msecs");
        // LongStream.rangeClosed
        System.out.println(measureSumPerf(StreamTest::rangedSum, 10_000_000) + " msecs");
        // parallel LongStream.rangeClosed
        System.out.println(measureSumPerf(StreamTest::parallelRangedSum, 10_000_000) + " msecs");
    }

    public static long parallelRangedSum(long n) {
        return LongStream.rangeClosed(1, n)
                .parallel()
                .reduce(0L, Long::sum);
    }

    public static long rangedSum(long n) {
        return LongStream.rangeClosed(1, n)
                .reduce(0L, Long::sum);
    }


    public static long parallelSum(long n) {
        return Stream.iterate(0L, i -> i + 1)
                .limit(n)
                .parallel()
                .reduce(0L ,Long::sum);
    }


    public static long sequentialSum(long n) {
        // 生成自然数无限流
        return Stream.iterate(0L, i -> i + 1)
                // 限制到前n个数
                .limit(n)
                // 对所有数字求和来归纳流
                .reduce(0L, Long::sum);
    }

    public static long iterativeSum(long n) {
        long result = 0;
        for (long i = 1L; i <= n; i++) {
            result += i;
        }
        return result;
    }

    public static long measureSumPerf(Function<Long, Long> adder, long n) {
        long fastest = Long.MAX_VALUE;
        for (int i = 0; i < 10; i++) {
            long start = System.nanoTime();
            long sum = adder.apply(n);
            long duration = (System.nanoTime() - start) / 1_000_000;
            if (duration < fastest) {fastest = duration;}
        }
        return fastest;
    }

}

```



## 15. 异步回调CompletableFuture

[CSDN-Java8 CompletableFuture 用法全解](https://blog.csdn.net/qq_31865983/article/details/106137777)

![1635235954180](images/1635235954180.png)

```java
package com.atguigu.completable;

import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

//异步调用和同步调用
public class CompletableFutureDemo {
    public static void main(String[] args) throws Exception {
        //同步调用
        CompletableFuture<Void> completableFuture1 = CompletableFuture.runAsync(()->{
            System.out.println(Thread.currentThread().getName()+" : CompletableFuture1");
        });
        completableFuture1.get();

        //mq消息队列
        //异步调用
        CompletableFuture<Integer> completableFuture2 = CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread().getName()+" : CompletableFuture2");
            //模拟异常
            int i = 10/0;
            return 1024;
        });
        completableFuture2.whenComplete((t,u)->{
            System.out.println("------t="+t);
            System.out.println("------u="+u);
        }).get();

    }
}
```

## 16. JMM

> 什么是JMM

JMM Java内存模型，不存在的东西，概念！约定！

关于JMM的一些同步的约定：

1. 线程解锁前，必须把共享变量立刻刷回主存
2. 线程加锁前，必须读取主存中的最新值到工作内存中
3. 加锁和解锁是同一把锁

> JMM8大原子操作

```java
1. read(读取)：
   从主存中读取数据
2. load(载入)：
   将主存读取到的数据写入工作内存中
3. use(使用)：
   从工作内存读取数据做计算
4. assign(赋值)：
   将计算好的值重新赋值到工作内存中
5. store(存储)：
   将工作内存数据写入主存
6. write(写入)：
   将store过去的变量值赋值给主存中的变量(更新主存)
7. lock(锁定)：
   将主存变量加锁，标识为线程独占状态
8. unlock(解锁)：
   将主存变量解锁，解锁以后其它线程就可以锁定该变量。
```

## 17. Volatile

1. 保证可见性

```java
// 不加volatile会导致死循环
public class VolatileTest {
    private  volatile static int num=0;
    public static void main(String[] args) {
        new Thread(()->{
            while(num==0){ }
        }).start ();
        try {
            TimeUnit.SECONDS.sleep ( 2 );
        } catch (InterruptedException e) {
            e.printStackTrace ();
        }
        num=2;
    }
}
```



2. 不保证原子性

```java
public class VolatileTest2 {
    private static volatile int num=0;

    public static void add(){
        num++;
    }
    public static void main(String[] args) {
        for(int i=1;i<=20;i++){
            new Thread ( ()->{
                for(int j=0;j<1000;j++){
                    add();
                }
            } ).start ();
        }
        while(Thread.activeCount ()>2){
            Thread.yield ();
        }
        System.out.println (Thread.currentThread ().getName ()+" "+num);
    }
}
```

> 如果不用synchronized和lock，如何保证原子性？

[CSDN-Java并发编程-无锁CAS与Unsafe类及其并发包Atomic](https://blog.csdn.net/javazejian/article/details/72772470)

使用Atomic

这个类底层都直接和操作系统挂钩！直接从内存改值！Unsafe类是一个很特殊的存在

![1635239282126](images/1635239282126.png)

```java
public class VolatileTest3 {
    private static volatile AtomicInteger num=new AtomicInteger (  );

    public static void add(){
        num.getAndIncrement ();
    }
    public static void main(String[] args) {
        for(int i=1;i<=20;i++){
            new Thread ( ()->{
                for(int j=0;j<1000;j++){
                    add();
                }
            } ).start ();
        }
        while(Thread.activeCount ()>2){
            Thread.yield ();
        }
        System.out.println (Thread.currentThread ().getName ()+" "+num);
    }
}

```

3. 禁止指令重排

指令重排：写的程序，计算机并不是按照你写的顺序执行

编译器优化的重排、指令并行的重排、内存系统的重排

处理器在进行指令重排时，会考虑数据之间的依赖性

| 线程A | 线程B |
| ----- | ----- |
| x=a   | y=b   |
| b=1   | a=2   |

正常的结果：x=0,y=0,但是可能由于指令重排导致诡异结果：x=2，y=1

| 线程A | 线程B |
| ----- | ----- |
| b=1   | a=2   |
| x=a   | y=b   |

volatile可以避免指令重排，因为在上下加内存屏障

![1635239970441](images/1635239970441.png)



## 19. 深入理解CAS

> 什么是CAS

CAS：比较当前工作内存中的值和主内存中的值，如果这个值是期望的，那么执行操作，如果不是就一直循环！

**缺陷**

1. 循环会耗时
2. 一次性只能保证一个共享变量的原子性
3. ABA问题

![1635243207831](images/1635243207831.png)

![1635243520276](images/1635243520276.png)

这里就是自旋锁

> CAS：ABA问题（狸猫换太子）

![1635243874738](images/1635243874738.png)

![1635243945256](images/1635243945256.png)

```java
public class CASDemo {

    public static void main(String[] args) {
        // ABA问题
        AtomicInteger atomicInteger=new AtomicInteger (2020  );
        // ====捣乱的线程====
        System.out.println ( atomicInteger.compareAndSet ( 2020, 2021 ) );
        System.out.println (atomicInteger.get ());
        System.out.println ( atomicInteger.compareAndSet ( 2021, 2020 ) );
        System.out.println (atomicInteger.get ());
        // ====期望的线程====
        System.out.println ( atomicInteger.compareAndSet ( 2020, 2021 ) );
        System.out.println (atomicInteger.get ());
    }
}

```



## 20. 原子引用

![1635245904574](images/1635245904574.png)

> 解决ABA问题，对应的思想：乐观锁

带版本的原子操作

```java
public class CASDemo {

    public static void main(String[] args) {
        // ABA问题
        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference<> ( 123, 1 );
        new Thread(()->{
            int stamp=atomicStampedReference.getStamp ();
            System.out.println ("a1="+stamp);
            try {
                TimeUnit.SECONDS.sleep ( 1 );
            } catch (InterruptedException e) {
                e.printStackTrace ();
            }
            atomicStampedReference.compareAndSet ( 123,124 ,atomicStampedReference.getStamp (),atomicStampedReference.getStamp ()+1);
            System.out.println ("a2="+atomicStampedReference.getStamp ());
            atomicStampedReference.compareAndSet ( 124,123 ,atomicStampedReference.getStamp (),atomicStampedReference.getStamp ()+1);
            System.out.println ("a3="+atomicStampedReference.getStamp ());
        }).start ();
        new Thread(()->{
            int stamp=atomicStampedReference.getStamp ();
            System.out.println ("b1="+stamp);
            try {
                TimeUnit.SECONDS.sleep ( 2 );
            } catch (InterruptedException e) {
                e.printStackTrace ();
            }
            atomicStampedReference.compareAndSet ( 123,125 ,stamp, stamp+1);
            System.out.println ("b2="+atomicStampedReference.getStamp ());

        }).start ();
    }
}

```
