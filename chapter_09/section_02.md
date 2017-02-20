# 并发

## 1. 问题的提出

当多个线程同时执行同一段代码时，可能引起并发问题，例如：

```java
public class TestConcurrent {
  public static int count = 0;
  public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
      for (int i = 0; i < 1000; i++) {
        count++;
      }
    });
    Thread t2 = new Thread(() -> {
      for (int i = 0; i < 1000; i++) {
        count--;
      }
    });
    t1.start();
    t2.start();
    t1.join(); // 等待t1运行结束(这时t2在同时运行)
    t2.join(); // 等待t2运行结束
    System.out.println(count);
  }
}
```
正如以上代码所示，一个线程对static变量count做了一千次自增，另一个线程对它做了一千次自减，但运行结果出人意料：有时候是0，有时候是负数，还有时候是正数。

是什么原因导致以上问题的发生呢？

首先明确一点，jvm中每个线程会使用独立的栈内存空间，局部变量，方法参数，返回值，都是使用这块空间。对于上面的例子，for循环中的int i是一个局部变量，在t1线程栈中的i 与t2线程栈中的i是完全不同的两个变量，它们不可能有交集，互不影响。
> 注意: 这里引申出的一个要点就是方法内的变量或对象，它们的作用范围要是仅局限于方法内，那么它们是线程安全的。

而` static int count ` 不同：它被多个线程所共享，这就有可能带来问题。上面的例子中` static int count `使用的是方法区内存，因此线程t1 要使用它，必须先通过jvm 指令将其读取到t1 栈内存空间，再执行自增指令，最后将结果写回方法区内存。底层对应要执行3条jvm指令：
```
getstatic
iadd
putstatic
```
对于线程t2也是如此。

线程t1 和 线程t2 是并行执行的，假设这些指令的执行顺序如下：
```
t1 - getstatic  这时count为0
t1 - iadd       在栈中运行将结果自增为1
t2 - getstatic  这时t1还没有将结果写回count，因此count还是为0
t2 - iadd       在0的基础上自减，结果为-1
t1 - putstatic  将t1的结果1写回count
t2 - putstatic  将t2的结果-1写回count 
最终结果为-1
```
可以看到两个线程分别做了一次自增和自减，但结果已经不正确了。指令的交错顺序不同，就会导致不同的结果产生。

实际运行时，一次自增自减运行非常快，可能不会出现指令交错并行的情况，因此需要增加循环次数或设置随机延时时间来增加交错并行的几率。

> 注意：一个变量或对象如果被多线程共享，那么它可能线程安全，可能线程不安全。引申出第二个要点：
  * 基本类型是final的共享变量线程安全
  * 引用类型的对象如果是不可变的，线程安全（如String）

## 2. 问题的解决
java中可以用` synchronized `关键字来保证多行代码的原子性，即受synchronized 保护的多行代码同一时刻只能被一个线程执行，其它线程只能排队等待。换句话说，synchronized 将并行操作变成了串行操作（也称为同步操作）。

### 1) 语法
```java
synchronized(obj) {
   // 多行要原子操作的代码
}
```
其中obj可以是任何对象，但要注意这个对象应该是能够为多个线程所共享和访问到的。

例如，如果要让之前的例子成功运行，可以将代码修改为：
```java
public class TestConcurrent {
  public static int count = 0;
  public static final Object mutex = new Object();
  public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
      for (int i = 0; i < 1000; i++) {
        synchronized (mutex) {
          count++;
        }
      }
    });
    Thread t2 = new Thread(() -> {
      for (int i = 0; i < 1000; i++) {
        synchronized (mutex) {
          count--;
        }
      }
    });
    t1.start();
    t2.start();
    t1.join(); // 等待t1运行结束(这时t2在同时运行)
    t2.join(); // 等待t2运行结束
    System.out.println(count);
  }
}
```
如何理解呢：你可以把mutex想象成一个房间，线程t1,t2想象成两个人。

当线程t1执行到 synchronized(mutex) 时就好比t1进入了这个房间，并反手锁住了门，在门内执行count++代码。

这时候如果t2 也运行到了 synchronized(mutex) 时，它发现门被锁住了，只能在门外等待。

当t1执行完synchronized{} 块内的代码，这时候才会解开门上的锁，从mutex房间出来。t2线程这时才可以进入mutex房间，反锁住门，执行它的count--代码。

> 注意：上例中t1和t2线程必须用 synchronized 锁住同一个mutex 对象，如果t1锁住的是m1对象，t2锁住的是m2对象，就好比两个人分别进入了两个不同的房间，没法起到同步的效果。


