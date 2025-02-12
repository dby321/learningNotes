

# JavaSE高级

[尚硅谷-宋红康JavaSE高级篇](https://www.bilibili.com/video/BV1Kb411W75N)

## 多线程

### 多线程的实现

> 开发中优先选择实现Runnable接口的方式
> 1. 实现的方式没有的类的单继承性的局限
> 2. 实现的方式更适合来处理多个线程有共享数据的情况



[线程创建-使用线程池](E:\idea_workspace\2020Study-JavaSE\01.Thread\src\com\demo10)

### 多线程的生命周期

> Thread.State内部类中定义了线程的生命周期

从源代码，总结出Java线程的状态有以下几种：

- **NEW**：尚未启动的线程状态。
  - 也称为初始状态、开始状态。

- **RUNNABLE**：可以运行的线程状态。
  - 线程已在JVM中运行，但可能在等待其他系统资源。
  - 也称为就绪状态、可运行状态。

- **BLOCKED**：线程因等待监视锁而被阻塞的状态。
  - 也称为阻塞状态。

- **WAITING**：正在等待的线程状态。
  - 也称为等待状态。
  - 造成线程等待的原因有三种：
    1. 调用 `Object.wait()`
    2. 调用 `join()`
    3. 调用 `LockSupport.park()`
  - 处于等待状态的线程正在等待其他线程执行特定操作。
    - 例如：因 `wait()` 而等待的线程正在等待另一个线程调用 `notify()` 或 `notifyAll()`。
    - 因 `join()` 而等待的线程正在等待另一个线程结束。

- **TIMED_WAITING**：在限定时间内等待的线程状态。
  - 也称为限时等待状态。
  - 造成线程限时等待状态的原因有五种：
    1. `Thread.sleep(long)`
    2. `Object.wait(long)`
    3. `join(long)`
    4. `LockSupport.parkNanos(obj, long)`
    5. `LockSupport.parkUntil(obj, long)`

- **TERMINATED**：完全运行完成的线程状态。
  - 也称为终止状态、结束状态。

### 线程的同步[线程安全问题]

[线程同步-卖票问题](E:\idea_workspace\2020Study-JavaSE\01.Thread\src\com\demo6)

> 多个线程执行的不确定性引起执行结果的不稳定。
>
> 典型就是卖票过程中会出现重票和错票问题。

[CSDN-线程安全问题的3种处理方式](https://blog.csdn.net/qq_38225558/article/details/82154321)

`synchronized`

> 1. 多个线程操作共享数据的代码，就是需要被同步的代码
> 2. 多个线程必须要共用同一把锁

> `window1.class`也可以充当锁，因为类也是对象
>
> `Class clazz=window1.class`

> `class Window4 extends Thread`时，如果想用同步方法，必须用static修饰，此时同步监视器默认是`Window4.class`
>
> `class Window3 implements Runnable`时，直接只用`synchronized`修饰要同步的方法即可，此时同步监视器默认是`this` 



### 线程的死锁

> 有两个锁嵌套时会出现死锁。
>
> 出现死锁后，不会出现异常，不会出现提示，所有线程处于阻塞状态。
>
> 写程序要避免死锁的出现。

### 线程的通信

>  `notify()` `wait()` `notifyAll()`**必须使用在同步代码块和同步方法中，他们的调用者必须是同步监视器**，否则会出现`IllegalMonitarStateException`异常。

[线程通信-生产者和消费者案例](E:\idea_workspace\2020Study-JavaSE\01.Thread\src\com\demo8)

 

## Java常用API

### String

![image-20201023141909677](.\images\image-20201023141909677.png)

> 通过字面量和new+构造器方式创建String有一些区别。

![image-20201023144210097](.\images\image-20201023144210097.png)

[字符串拼接](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\string\demo2)

> 字符串拼接时
>
> 1. 常量与常量的拼接结果在常量池，且常量池中不会存在相同内容的常量
> 2. 只要拼接的其中有一个是变量，结果就在堆中。

[String常用方法](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\string\demo3)

[String和其他类型的转换](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\string\demo4)

### StringBuffer和StringBuilder

[StringBuffer和StringBuilder](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\string\demo5)

> `StringBuffer`线程安全，速度慢
>
> `StringBuilder`线程不安全，速度快
>
> 两者都是可变的字符序列，而`String`是不可变的字符序列

> `StringBuffer` `StringBuilder` `String` 底层都是通过char[]存储的
>
> 运行速度上`String`最慢

### Date和SimpleDateFormat

[Date的使用](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\datetime\demo1)

[SimpleDateFormat的使用](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\datetime\demo2)

### Calendar

[Calendar的使用](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\datetime\demo3)

### Java8中的java.time

[java.time的使用](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\datetime\demo4)

### Java比较器

[要排序的类实现Comparable接口-一劳永逸](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\compare\demo1)

[定制排序Comparator接口-灵活定制](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\compare\demo2)

### System

[System](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\system\demo1)

### Math

[Math](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\math\demo1)

### BigInteger和BigDecimal

[BigInteger和BigDecimal](E:\idea_workspace\2020Study-JavaSE\02.JavaAPI\src\com\big\demo1)

## 枚举和注解

### 枚举

[枚举](E:\idea_workspace\2020Study-JavaSE\03.EnumZhujie\src\com\enummm\demo1)

### 注解

```java
package java.lang.annotation;

public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */

    FIELD,              /* 字段声明（包括枚举常量）  */

    METHOD,             /* 方法声明  */

    PARAMETER,          /* 参数声明  */

    CONSTRUCTOR,        /* 构造方法声明  */

    LOCAL_VARIABLE,     /* 局部变量声明  */

    ANNOTATION_TYPE,    /* 注释类型声明  */

    PACKAGE             /* 包声明  */
}
```

```java
package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */

    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */

    RUNTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

**(01) @interface**

使用 @interface 定义注解时，意味着它实现了 java.lang.annotation.Annotation 接口，即该注解就是一个Annotation。

定义 Annotation 时，@interface 是必须的。

注意：它和我们通常的 implemented 实现接口的方法不同。Annotation 接口的实现细节都由编译器完成。通过 @interface 定义注解后，该注解不能继承其他的注解或接口。

**(02) @Documented**

类和方法的 Annotation 在缺省情况下是不出现在 javadoc 中的。如果使用 @Documented 修饰该 Annotation，则表示它可以出现在 javadoc 中。

定义 Annotation 时，@Documented 可有可无；若没有定义，则 Annotation 不会出现在 javadoc 中。

**(03) @Target(ElementType.TYPE)**

前面我们说过，ElementType 是 Annotation 的类型属性。而 @Target 的作用，就是来指定 Annotation 的类型属性。

@Target(ElementType.TYPE) 的意思就是指定该 Annotation 的类型是 ElementType.TYPE。这就意味着，MyAnnotation1 是来修饰"类、接口（包括注释类型）或枚举声明"的注解。

定义 Annotation 时，@Target 可有可无。若有 @Target，则该 Annotation 只能用于它所指定的地方；若没有 @Target，则该 Annotation 可以用于任何地方。

**(04) @Retention(RetentionPolicy.RUNTIME)**

前面我们说过，RetentionPolicy 是 Annotation 的策略属性，而 @Retention 的作用，就是指定 Annotation 的策略属性。

@Retention(RetentionPolicy.RUNTIME) 的意思就是指定该 Annotation 的策略是 RetentionPolicy.RUNTIME。这就意味着，编译器会将该 Annotation 信息保留在 .class 文件中，并且能被虚拟机读取。

定义 Annotation 时，@Retention 可有可无。若没有 @Retention，则默认是 RetentionPolicy.CLASS。

## 集合

### Collection中的List和Set

[List和Set](E:\idea_workspace\2020Study-JavaSE\04.Collection\src\com\collection\demo1)

### Map

[Map](E:\idea_workspace\2020Study-JavaSE\04.Collection\src\com\map\demo1)

### Collections

[Collections工具类](E:\idea_workspace\2020Study-JavaSE\04.Collection\src\com\collections\demo1)

## 泛型

[泛型和泛型通配符](E:\idea_workspace\2020Study-JavaSE\05.FanXing\src\com\generic\demo1)

## 文件操作

### File

[File](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\file\demo1)

### IO流

![image-20201026182903374](.\images\image-20201026182903374.png)

[主要的IO流](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\io\demo1)

[其他的IO流1](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\io\demo2)

[其他的IO流2](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\io\demo3)

### 字符集

略

![image-20201027203757530](.\images\image-20201027203757530.png)

### NIO和NIO2

> 核心API包括Path,Paths,Files
>
> 具体到框架时细讲。

[NIO和NIO2](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\nio\demo1)

### CommonIO

> common-io是apache提供的操作文件的工具包，底层还是File和IO流

[common-io](E:\idea_workspace\2020Study-JavaSE\06.IO\src\main\java\com\commonio\demo1)

## 网络编程

[InetAddress Socket ServerSocket DatagramSocket DatagramPacket URL](E:\idea_workspace\2020Study-JavaSE\07.NetworkPrograming\src\main\java\com\demo1)

## 反射

[反射的使用](E:\idea_workspace\2020Study-JavaSE\08.Reflection\src\com\demo1)

![image-20201028201625950](.\images\image-20201028201625950.png)

![image-20201028201639096](.\images\image-20201028201639096.png)

> 类本身也是Class的一个对象

![image-20201028203825211](.\images\image-20201028203825211.png)

> 类加载器具体参见JVM教程

## 动态代理

> 动态代理是反射的应用

[静态代理和动态代理](E:\idea_workspace\2020Study-JavaSE\09.DongTaiDaiLi\src\com\demo1)

## JDK8新特性

[CSDN-Java 8的新特性](https://blog.csdn.net/yczz/article/details/50896975)

[Lambda表达式 方法引用](E:\idea_workspace\2020Study-JavaSE\10.Java8\src\com\demo1)

[Stream Optional](E:\idea_workspace\2020Study-JavaSE\10.Java8\src\com\demo2)

## 序列化与反序列化

[CSDN-Jackson，Gson，Fastjson 的对比](https://blog.csdn.net/qidasheng2012/article/details/82996405)

