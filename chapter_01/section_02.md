# 环境变量

## 1. JAVA\_HOME 环境变量

Java程序运行之前需要知道JDK或JRE的安装路径，这个路径就是JAVA\_HOME.

例如：eclipse, tomcat, solr 这些程序在运行时，都需要JAVA\_HOME

### 1.1 查看JAVA\_HOME环境变量是否配置正确

Windows：
```
cmd > echo %JAVA_HOME%
```

Linux：
```
shell > echo $JAVA_HOME
```

### 1.2 临时修改JAVA_HOME环境变量

Windows：
```
cmd > set JAVA_HOME=安装目录
```

Linux：
```
shell > export JAVA_HOME=安装目录
```

> 注意：用以上方法修改，当cmd或shell窗口关闭时则失效

### 1.3 永久修改JAVA_HOME环境变量

Windows：
在`系统`->`高级系统设置`->`环境变量`中设置

Linux：
```
shell > vi /etc/profile
shift+g
end
a
export JAVA_HOME=安装目录
:wq
shell > source /etc/profile
```

## 2. PATH 环境变量
PATH 环境变量决定了在哪些路径下搜索`可执行程序`和`脚本`

Path可以由多个路径组成，多个路径之间：

Windows下用;分隔

Linux下用:分隔


## 3. CLASSPATH
CLASSPATH 环境变量决定了在哪些路径下可以搜索到`java 类`



