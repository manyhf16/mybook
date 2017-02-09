# 线程通信

更多的时候两个线程要协同工作，要进行线程间的通信。

例如：两个线程同时启动，如何保证一个线程一定先执行，另一个后执行？

刚才的场景用代码可以写成：

```
public class TestThread2 {
  static Object mutex = new Object();
  public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
      synchronized (mutex) {
        try {
          mutex.wait();
        } catch (Exception e) {
        }
      }
      System.out.println(1);
    });
    Thread t2 = new Thread(() -> {
      System.out.println(2);
      synchronized (mutex) {
        mutex.notify();
      }
    });
    t1.start();
    t2.start();
  }

}
```