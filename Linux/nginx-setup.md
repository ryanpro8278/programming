# Nginx在Centos下的安装

## 介绍

Nginx是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器，作为Http服务器，有以下几项基本特征：  

1 处理静态文件,索引文件以及自动索引，打开文件描述符缓冲。  

2 无缓存的反向代理加速，简单的负载均衡和容错  

3 模块化的结构，包括gzipping，byte ranges,chunked responses以及SSI-filter等filter，如果由FastCGI或其它代理服务器处理蛋液中存在的多个SSI,则这项处理可以并行运行，而不需要相互等待。  

4 支持SSL和TLSSNI。  

Nginx官网：http://nginx.org/  

Nginx推荐学习网址：http://dreamfire.blog.51cto.com/418026/1140965

## 环境

Nginx的安装依赖 **openssl** 库、**zlib** 库以及 **pcre** 库的支持，请见以下所示：  

**nginx-1.11.6.tar.gz** ，下载地址：[http://nginx.org/](http://nginx.org/)  

SSL功能需要的 **openssl-1.1.0c.tar.gz** ，下载地址：[http://www.openssl.org/](http://www.openssl.org/)  

gzip模块需要的 **zlib-1.2.8.tar.gz** ，下载地址：[http://www.zlib.net/](http://www.zlib.net/)  

rewrite模块需要的 **pcre-8.39.tar.gz** ，下载地址：[http://www.pcre.org/](http://www.pcre.org/)  

## 步骤

### 1 安装openssl库以及zlib库

a. 安装SSL功能需要的openssl库插件

```bash
[root@master download]# tar zxvf openssl-1.1.0c.tar.gz 
[root@master download]# cd openssl-1.1.0c
[root@master openssl-1.1.0c]# ./config 
[root@master openssl-1.1.0c]# make
[root@master openssl-1.1.0c]# make install
```

b. 安装gzib模块需要的zlib库插件

```bash
[root@master download]# tar xvf zlib-1.2.8.tar.gz 
[root@master download]# cd zlib-1.2.8
[root@master zlib-1.2.8]# ./configure 
[root@master zlib-1.2.8]# make
[root@master zlib-1.2.8]# make install
```

### 2 安装pcre库

a. 安装rewrite模块需要的pcre库

```bash
[root@master download]# tar zxvf pcre-8.39.tar.gz 
[root@master download]# cd pcre-8.39
[root@master pcre-8.39]# ./configure 
[root@master pcre-8.39]# make
[root@master pcre-8.39]# make install
```

b. 可能会遇到 `error: You need a C++ compiler for C++ support` 的错误，说明需要安装C++包

```bash
yum install -y gcc gcc-c++
```

### 3 安装Nginx服务

安装Nginx的时候需要附上上面已经安装好的3个库，具体附加步骤参见 **./configure** 即可

```bash
[root@master download]# tar zxvf nginx-1.11.6.tar.gz 
[root@master download]# cd nginx-1.11.6
[root@master nginx-1.11.6]# ./configure --with-pcre=../pcre-8.39 --with-zlib=../zl
ib-1.2.8 --with-openssl=../openssl-1.1.0c
[root@master nginx-1.11.6]# make 
[root@master nginx-1.11.6]# make install

```

###  4 检测Nginx是否安装成功

a.  进入nginx目录检测是否安装成功

```bash
[root@master nginx-1.11.6]# cd /usr/local/nginx/sbin/
[root@master sbin]# ./nginx -t
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```

b. 可能遇到的问题如权限不够需要添加权限

```bash
chmod -R 777 usr/local/nginx
```

+ 或者是碰到 `nginx: [error] invalid PID number "" in "/usr/local/nginx/logs/nginx.pid"` 时需要添加 **nginx.pid** 文件

```bash
[root@master sbin]# ./nginx -s reload
nginx: [error] invalid PID number "" in "/usr/local/nginx/logs/nginx.pid"
[root@master sbin]# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
[root@master sbin]# ./nginx -s reload
```

c. 更改防火墙设置，开放80端口

```bash
[root@master sbin]# vim /etc/sysconfig/iptables
...
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
...
[root@master sbin]# /etc/init.d/iptables restart
iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
```

d. 在外部浏览器中浏览：**localhost** ，出现如图所示则说明安装已经全部完成

![](http://ww1.sinaimg.cn/large/82c8e86egw1fbdtrng8nkj20q50ac0uu.jpg)

### 5 nginx的开机启动与chkconfig管理

a. 首先在 **/etc/init.d/** 目录下创建nginx文件，使用

```bash
vim /etc/init.d/nginx
```

+ 添加如下官方脚本，脚本地址：[http://wiki.nginx.org/RedHatNginxInitScript](http://wiki.nginx.org/RedHatNginxInitScript) ，注意如果是自定义编译安装的nginx，需要根据安装路径修改下面这两项配置：

  **nginx=”/usr/sbin/nginx”** 修改成nginx执行程序的路径。

  **NGINX_CONF_FILE=”/etc/nginx/nginx.conf”** 修改成配置文件的路径。
```bash
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemon
#
# chkconfig:   - 85 15
# description:  NGINX is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /etc/nginx/nginx.conf
# config:      /etc/sysconfig/nginx
# pidfile:     /var/run/nginx.pid
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/etc/nginx/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
make_dirs() {
   # make required directories
   user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   if [ -z "`grep $user /etc/passwd`" ]; then
       useradd -M -s /bin/nologin $user
   fi
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
       if [ `echo $opt | grep '.*-temp-path'` ]; then
           value=`echo $opt | cut -d "=" -f 2`
           if [ ! -d "$value" ]; then
               # echo "creating" $value
               mkdir -p $value && chown -R $user $value
           fi
       fi
   done
}
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    make_dirs
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
force_reload() {
    restart
}
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
    status $prog
}
rh_status_q() {
    rh_status >/dev/null 2>&1
}
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```
+ 保存脚本文件后**设置文件的执行权限：**

```bash
chmod a+x /etc/init.d/nginx
```

+ 然后，就可以通过该脚本对nginx服务进行管理了：

```bash
/etc/init.d/nginx start
/etc/init.d/nginx stop
```

b. 上面的方法完成了用脚本管理nginx服务的功能，下一步则可以利用chkconfig来设置Nginx的开机启动

+ 先将nginx服务加入chkconfig管理列表：

```bash
chkconfig --add /etc/init.d/nginx
```

+ 加完这个之后，就可以使用service对nginx进行启动，重启等操作了。

```bash
service nginx start
service nginx stop
```

+ 设置终端模式开机启动：

```bash
chkconfig nginx on
```
结束安装咯！



参考：  

http://www.cnblogs.com/hanyinglong/p/5102141.html#_label7  

http://blog.csdn.net/u013870094/article/details/52463026