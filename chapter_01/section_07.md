# 注释

***
## 1. 注释
所谓注释，不影响程序运行，只起到提示程序员的作用。换个角度说，注释只存在于 \*.java 文件中，在javac编译之后的 \*.class 文件中，注释已经消失了。

> 注意：注释可以加在代码的任意位置

## 2. 单行注释
例如：

```
// 这是单行注释1
public class HelloWorld {
    // 这是单行注释2
    public static void main(String[] args) {
        // 这是单行注释3
        System.out.println("你好，世界！"); 
    }

}
```

## 3. 多行注释
```
/* 
 *  这是多行注释，第一行
 *  第二行
 *  第三行
 */
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("你好，世界！"); 
    }

}
```

## 4. 文档注释
```
/** 
 *  这是文档注释，第一行
 *  第二行
 *  第三行
 */
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("你好，世界！"); 
    }

}
```
文档注释的格式是 `/** 注释内容 */`，而多行注释的格式是`/* 注释内容 */`

除此以外，文档注释主要加在public 的类(class)、方法(method)、和域(field)上，可以配合`javadoc 命令`生成html格式的java api文档
***