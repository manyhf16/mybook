# 线程通信

更多的时候两个线程要协同工作，要进行线程间的通信。

## 问题提出

例如：两个线程同时启动，如何保证一个线程一定先执行，另一个后执行？

刚才的场景用代码可以写成：

```
public class TestSignal {
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

## 第一次改进

```
public class TestSignal {
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

这样，如果t1 先运行，运行到mutex时，就进入mutex等待； 接下来t2 运行，打印了2 以后，进入mutex唤醒了在mutex上等待了 t1 线程，t1 线程恢复运行，打印1，这时顺序是正确的。

但如果t2 先运行，打印了2 以后，进入mutex唤醒mutex上等待的线程，而这时mutex上根本没有等待的线程，所以接下来t1运行到mutex时，进入mutex等待，将来就没人来唤醒它了。

## 第二次改进

需要加一个标记位来标记t1 是否需要wait()，前面已经分析过，如果t2已启动，这时的顺序已经达到了之前的要求，t1不需要wait()，否则t1才需要wait()：

```
public class TestSignal {
  static Object mutex = new Object();
  static boolean t2Started = false;
  public static void main(String[] args) {
    Thread t1 = new Thread(() -> {      
      synchronized (mutex) {
        if(! t2Started) {
          try {
            mutex.wait();
          } catch (Exception e) {
          }
        }
      }
      System.out.println(1);
    });
    Thread t2 = new Thread(() -> {
      System.out.println(2);
      synchronized (mutex) {
        t2Started = true;
        mutex.notify();
      }
    });
    t1.start();
    t2.start();
  }
}
```

目前的代码还是有点问题：如果这时又加入其它线程的干扰，唤醒了
