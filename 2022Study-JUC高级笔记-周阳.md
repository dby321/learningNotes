# JUC高级笔记

## 线程基础知识复习

### 线程启动后做了什么

java线程通过start方法启动执行的，主要内容在native方法start0中

openjdk的写JNI一般是一一对应的，Thread.java对应的就是Thread.c

start0其实就是JVM_StartThread。此处查看源代码可以看到在jvm.h中找到了声明，jvm.cpp中有实现

![1655378581341](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655378581341.png)

是JVM配合操作系统底层分配了一个基础线程

## CompletableFuture

### Future接口理论知识复习

Future接口（FutureTask是实现类）定义了操作异步任务执行一些方法

![1655380156346](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655380156346.png)

当我们需要三个特点：多线程，有返回，异步任务,且线程构造器Thread(Runnable target,String name)

就需要传入FutureTask(Callable<T> callable)

```java
public class FutureTaskTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<String> futureTask=new FutureTask<>(new MyThread());
        Thread thread=new Thread(futureTask,"t1");
        thread.start();
        System.out.println(futureTask.get());
    }
}
class MyThread implements Callable<String>{

    @Override
    public String call() throws Exception {
        return "hello call()";
    }
}
```

### Future的优缺点分析-->CompletableFuture

优点： 能够减少程序的运行时间,因为能够同时跑很多个线程并接收返回值. 

缺点：get()是阻塞的，一直等到有结果返回，与异步编程的设计理念相违背；isDone()会不断轮询，并且不能及时获取,耗费无谓的CPU资源

### CompletableFuture

CompletionStage代表异步的某个阶段

构造器没有指定线程池，默认是ForkJoinPool.commonPool(),它是守护线程

join()不抛出异常

### 链式调用

 在实体类上加`@Accessor(chain=true)`

![1655439825965](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655439825965.png)



### CompletableFuture的API总结

- 获取结果和主动触发计算
  - get()
  - get(long timeout,TimeUnit unit)
  - join()
  - getNow(T valueIfAbsent)
  - complete(T value) `If not already completed, sets the value returned by get() and related methods to the given value.`

- 对计算结果进行处理
  - thenApply(Function<? super T,? extends U> fn)
  - handle(BiFunction<? super T,Throwable,? extends U> fn)

- 对计算结果进行消费
  - thenAccept(Consumer<? super T> action)

- 对计算速度进行选用
  - applyToEither(CompletionStage<? extends T> other, Function<? super T,U> fn)

- 对计算结果进合并
  - thenCombine(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn)

## Java锁

### synchronized

synchronized同步代码块：底层是monitorenter和monitorexit

synchronized普通同步方法：flags处是ACC_SYNCHRONIZED

synchronized静态同步方法：flags处是ACC_STATIC和ACC_SYNCHRONIZED

### 管程Monitor

每个对象天生就带着一个对象监视器

被锁着的对象都会和monitor关联起来

## 线程中断

### 什么是中断机制与中断方法

Java提供了一种用于停止线程的协商机制——中断。

中断方法**interrupt()**仅仅是将中断标识位设置为true，发起一个协商而不会立即停止线程

**interrupted()**返回当前线程的中断状态，测试当前线程是否已被中断；将当前先测会给你的中断状态清零并重新设置为false。

**isinterrupted()**返回当前线程的中断状态

### LockSupport

park和unpark，许可证最多只有一个

## Java内存模型JMM

JMM来屏蔽掉各种硬件和操作系统的内存访问差异

![1655897276168](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655897276168.png)

happens-before的语义本质是一种可见性

## volatile与JMM

内存屏障：读屏障和写屏障，细分四种

内存屏障能禁止指令重排

 

## CAS

### 面试题：手写自旋锁

![1655901478930](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655901478930.png)

### AtomicStampedReference入门

![1655901443618](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655901443618.png)

### CAS缺点

循环时间长开销大

ABA问题

## 原子操作类

> 剩余部分参见javaguide

### 引用类型

**AtomicStampedReference**解决修改过几次

**AtomicMarkedReference**解决是否修改过

### 对象属性修改类型

![1655951945250](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655951945250.png)

![1655952294129](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655952294129.png)

### 原子操作增强类

**LongAddr**只能做初始值为0的加法运算，高并发下吞吐量很高，底层思想是分散热点，将value值分散到Cell数组中

![1655953694273](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655953694273.png)

![1655954415837](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655954415837.png)

**LongAccumulator**可以做初始值不为0的加减乘除运算

![1655952676247](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655952676247.png)

## ThreadLocal线程局部变量

### ThreadLocal的使用

![1655955399977](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655955399977.png)

![1655955898028](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655955898028.png)

### ThreadLocal的源码分析

![1655956157581](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655956157581.png)

![1655988989614](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655988989614.png)

  

### ThreadLocal内存泄露问题

什么是内存泄露？

谁惹的祸？

为什么要用弱引用，不用如何？

最佳实践

## Java对象内存布局和对象头

> JOL
>
> https://zhuanlan.zhihu.com/p/151856103?from_voters_page=true

对象头一般16字节，8字节Markword，8字节类型指针()

![1655990341749](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655990341749.png)

![1655990415203](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655990415203.png)

![1655990640822](C:\Users\waterplants\AppData\Roaming\Typora\typora-user-images\1655990640822.png)

## Synchronized和锁升级

