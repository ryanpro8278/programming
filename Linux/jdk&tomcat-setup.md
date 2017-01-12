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

### 3 设置Tomcat开机启动与chkconfig管理

a. 在 **/etc/init.d/** 下新建 **tomcat** ,写入下面的代码:

```bash
#!/bin/bash
#
# /etc/rc.d/init.d/tomcat
# init script for tomcat precesses
#
# processname: tomcat
# description: tomcat is a j2se server
# chkconfig: 2345 86 16
# description: Start up the Tomcat servlet engine.

if [ -f /etc/init.d/functions ]; then
. /etc/init.d/functions
elif [ -f /etc/rc.d/init.d/functions ]; then
. /etc/rc.d/init.d/functions
else
echo -e "/atomcat: unable to locate functions lib. Cannot continue."
exit -1
fi

RETVAL=$?
CATALINA_HOME="/usr/local/tomcat"

case "$1" in
start)
if [ -f $CATALINA_HOME/bin/startup.sh ];
then
echo $"Starting Tomcat"
$CATALINA_HOME/bin/startup.sh
fi
;;
stop)
if [ -f $CATALINA_HOME/bin/shutdown.sh ];
then
echo $"Stopping Tomcat"
$CATALINA_HOME/bin/shutdown.sh
fi
;;
*)
echo $"Usage: $0 {start|stop}"
exit 1
;;
esac

exit $RETVAL
```

+ 注意要将 **CATALINA_HOME的路径** 设为自己的 **Tomcat路径**  

b. 给 **tomcat** 文件添加权限与chkconfig

```bash
[root@master init.d]# chmod 755 tomcat 
[root@master init.d]# chkconfig --add tomcat 
```
c. 在 **tomcat/bin/catalina.sh** 文件中加入以下语句(路径跟随自己的实际情况更改)：

```
export JAVA_HOME=/usr/java/jdk1.8.0_111
export CATALINA_HOME=/usr/local/tomcat
export CATALINA_BASE=/usr/local/tomcat
export CATALINA_TMPDIR=/usr/local/tomcat/temp
```

d. 测试 **启动** 与 **停止** tomcat的命令

```bash
[root@master bin]# service tomcat start
Starting Tomcat
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Tomcat started.
[root@master bin]# service tomcat stop
Stopping Tomcat
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
```

完成安装！

参考：[http://blog.csdn.net/liuyan4794/article/details/16328077](http://blog.csdn.net/liuyan4794/article/details/16328077)