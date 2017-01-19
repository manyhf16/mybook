# Java程序开发的运行流程

***

## 1. 开发

使用任意的文本编辑器编写HelloWorld.java 源文件
```
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("你好，世界！");
    }

}
```

## 2. 编译

使用 javac 命令编译HelloWorld.java 源文件
```
cmd > javac -d . HelloWorld.java
```
结果会生成 HelloWorld.class 字节码文件
> 注意：-d . 是为了在当前目录下对带包的*.java生成对应的包目录

## 3. 运行
使用 java 命令运行 HelloWorld.class 字节码文件
```
cmd > java HelloWorld
```
