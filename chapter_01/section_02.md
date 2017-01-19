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

### 1.2 临时修改JAVA\_HOME环境变量

Windows：

```
cmd > set JAVA_HOME=安装目录
```

Linux：

```
shell > export JAVA_HOME=安装目录
```

> 注意：

## 2. Path 环境变量

## 3. ClassPath



