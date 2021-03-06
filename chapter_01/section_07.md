# 类型提升

---

## 1. 提升规则

当执行数值相关的运算（包括赋值运算和方法调用）时，会发生类型提升问题。
提升规则如下：

* 规则1： 小于等于int的变量提升为int
* 规则2： 有一个变量大于int，则提升为那个更大的类型
* 规则3： 小的类型可以直接赋值给大的类型，但反之不行
* 规则4： `增强赋值运算`和`自增自减`不会发生类型提升
* 规则5： 如果一个表达式全部由字面量组成，计算是发生在`编译阶段`，而不是`运行阶段`

`由小到大`的规律有两条线：

byte  &lt;  short  &lt;  int &lt; long &lt; float &lt; double

char &lt; int &lt; long &lt; float &lt; double

## 2. 示例

例1：

```java
byte a = 10;
byte b = 20;
int c = a + b;
```

a是byte，b是byte，执行a+b运算，按照规则1，它们都会提升为int

例2：

```java
int a = 10;
long b = 20L;
long c = a + b;
```
a是int，b是long，执行a+b运算，按照规则2，它们会提升为long

例3：

```java
short a = 10;
a = a + 20;    // 编译报错
```

第二行代码有问题： a 是short变量，但 a+20 发生了类型提升，且提升为int，int的结果不能直接赋值给short （规则3）


例4：

```java
short a = 10;
a += 20;
```

正确，根据规则4，+= 增强赋值运算符不会发生类型提升


例5：

```java
short a = 10 + 20;
```
10和20都是字面量，按照规则5，在编译时就计算出结果是30（注意这里没有把结果看做int），等价于：

```java
short a = 30;
```


例6：

```java
float a = 10 + 20.0f;
```
10和20.0f都是字面量，按照规则5，在编译时就计算出结果是30.0f（这里采用了类型提升规则），等价于：

```java
float a = 30.0f;
```




---



