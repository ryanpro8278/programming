JDK&Tomcat 在CentOS下的安装与配置

## 介绍

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。

由于Tomcat的运行需要Java环境的支持，所以首先需要安装Java开发工具包(JDK)

## 环境

jdk-8u111-linux-x64.rpm，可以从[Oracle官方](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载

apache-tomcat-8.5.9.tar.gz，可以从[Apache官方](http://tomcat.apache.org/download-80.cgi)下载

## 步骤

### 1 安装jdk

a. 将下载的jdk包导入Linux系统，进入安装目录进行安装

```bash
[root@master /]# cd /usr/local/download/
[root@master download]# rpm -ivh jdk-8u111-linux-x64.rpm 
Preparing...                ########################################### [100%]
   1:jdk1.8.0_111           ########################################### [100%]
Unpacking JAR files...
        tools.jar...
        plugin.jar...
        javaws.jar...
        deploy.jar...
        rt.jar...
        jsse.jar...
        charsets.jar...
        localedata.jar...
```

b. 设置环境变量

`#vi /etc/profile`

打开后，在文档最下方加上以下环境变量配置代码：

```
export JAVA_HOME=/usr/java/jdk1.8.0_111
export CLASSPATH=.:JAVA_HOME/lib/dt.jar:JAVA_HOME/lib/tools.jar
export PATH=JAVA_HOME/bin:PATH
```

注意：`export PATH=$JAVA_HOME/bin:$PATH`，注意将$PATH放到最后。以免造成新旧版本问题。

c. 检查JDK是否安装成功

```bash
[root@master etc]# java -version
java version "1.8.0_111"
Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)
```

如果看到JVM版本及相关信息，即安装成功！

### 2 安装Tomcat

a. 将下载的Tomcat导入Linux系统并解压

```bash
[root@master download]# tar zxvf apache-tomcat-8.5.9.tar.gz 
[root@master download]# mv apache-tomcat-8.5.9 /usr/local/tomcat
```

b. 启动Tomcat

```bash
[root@master download]# cd /usr/local/tomcat/bin/
[root@master bin]# ./startup.sh 
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Tomcat started.
```

c. 测试是否安装成功

访问：**http://localhost:8080** 出现如下说明已经启动

![](http://ww4.sinaimg.cn/large/82c8e86egw1fblycezhofj20q40eqtcw.jpg)