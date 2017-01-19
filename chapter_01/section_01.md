# Java的版本

---

## 1. Java三大平台\(Platform\)

| 名称 | 全称 | 旧名 |
| --- | --- | --- |
| JavaSE | Java Platform, Standard Edition | J2SE |
| JavaEE | Java Platform, Enterprise Edition | J2EE |
| JavaME | Java Platform, Micro Edition | J2ME |

## 2. Java SE

JavaSE的组成为：  
![](/chapter_01/1.jpg)

简单的说：

* JavaSE=JDK

* JDK=JRE+开发工具

* JRE=JVM+基础类库

* JVM=class的运行环境


截至今日，JDK的版本有

| 版本 | 发布时间 | 代号 | 重大功能 | 备注 |
| --- | --- | --- | --- | --- |
| 1.0 | 1996/01 |  |  | 最早叫做Oak |
| 1.1 | 1997/02 |  |  |  |
| 1.2 | 1998/08 | Playground |  |  |
| 1.3 | 2000/05 | Kestrel |  |  |
| 1.4 | 2002/02 | Merlin | 正则表达式、NIO... |  |
| 5.0 | 2004/09 | Tiger | 泛型、自动拆装箱、枚举、可变参数、for each增强 | 也叫1.5 |
| 6.0 | 2006/12 | Mustang |  | 也叫1.6 |
| 7.0 | 2011/07 | Dolphin | 对象排序算法由merge sort 改为timsort | 也叫1.7 |
| 8.0 | 2014/03 |  | lambda、stream API 永久代去除 | 也叫1.8 |
| 9.0 | 2017/03? |  |  |-  |

查看当前JDK版本的命令：  

```
cmd > java -version
```

## 3.Java EE

在Java SE的基础上，增加了企业级的功能，如Servlet,JSP,EJB等，但这些功能需要应用程序服务器（容器）的支持，如Tomcat,Jetty,JBoss等。

其中Tomcat和Jetty容器只支持Servlet和JSP规范，因此也称为WEB容器。

而JBoss和TomEE可以支持Servlet,JSP,EJB规范，因此也称为EJB容器。但EJB由于其门槛较高（主要是成本而非技术）在国内不是很流行。

Tomcat支持的规范版本如下：

| 版本 | 支持的JavaEE | 重大功能 | 备注 |
| --- | --- | --- | --- |
| 6.0 | JavaEE 5 | Servlet 2.5、JSP 2.1、Comet IO | JSP 2.1 已允许在表达式中调用方法 |
| 7.0 | JavaEE 6 | Servlet 3.0、JSP 2.2 |  |
| 8.0 | JavaEE 7 | Servlet 3.1、JSP 2.3、Unified EL、WebSocket、URIEncoding默认为UTF-8 |  |
| 8.5 | JavaEE 7 | 去除Comet、去除BIO |  |

***



