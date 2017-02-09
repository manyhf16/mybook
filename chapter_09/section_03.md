# 线程通信

更多的时候两个线程要协同工作，要进行线程间的通信。

例如：两个线程同时启动，如何保证一个线程一定先执行，另一个后执行？

刚才的场景用代码可以写成：

```
public class TestThread2 {
  public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
      System.out.println(1);
    });
    Thread t2 = new Thread(() -> {
      System.out.println(2);
    });
    t1.start();
    t2.start();
  }
}
```
多运行几次可观察到，有时打印 1 2， 有时打印 2 1，顺序是不一定的。

比如任务要求必须t2打印完毕2之后，再运行t1打印1，如何实现呢？

