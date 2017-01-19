# 环境变量

## 1. JAVA_HOME 环境变量

Java程序运行之前需要知道JDK或JRE的安装路径，这个路径就是JAVA_HOME.

例如：eclipse, tomcat, solr 这些程序在运行时，都需要JAVA_HOME

在Windows下查看该环境变量是否配置正确：
~~~
cmd > echo %JAVA_HOME%
~~~

在Linux下查看该环境变量是否配置正确：
~~~
shell > echo $JAVA_HOME
~~~

## 2. Path 环境变量
## 3. ClassPath