# 并发

当多个线程同时执行同一段代码时，可能引起并发问题，例如：

```
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
> 注意: 这里引申出的一个要点就是方法内声明的局部变量，包括在方法内新创建的对象，它们都是线程安全的。

而



