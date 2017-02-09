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

这时需要借助Object 中的wait()和notify()方法，调用：
* 对象.wait()方法是让当前线程在此对象上等待，线程代码会暂停运行。可以有多个线程在同一对象上进行等待
* 对象.notify()方法是在唤醒对象上随机一个等待的线程
* 对象.notifyAll()方法是唤醒对象上所有等待的线程

> 注意：wait() 和 notify() 方法运行前都必须先给这个对象加锁。