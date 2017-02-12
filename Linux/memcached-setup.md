# Memcached在Centos下的安装

## 介绍

Memcached 是一个高性能的分布式内存对象缓存系统，用于动态Web应用以减轻数据库负载。它通过在内存中缓存数据和对象来减少读取数据库的次数，从而提高动态、数据库驱动网站的速度。

## 环境

Memcached的安装依赖于libevent，因此需要准备的文件有：

[libevent-1.4.14b-stable.tar.gz](http://libevent.org/)

[memcached-1.4.15.tar.gz]([http://memcached.googlecode.com/files/memcached-1.4.15.tar.gz](http://memcached.googlecode.com/files/memcached-1.4.15.tar.gz))

## 步骤

### 1 安装Memcached缓存框架

a. 安装Memcached所需要的libevent库

```bash
[root@master download]# tar zxvf libevent-1.4.14b-stable.tar.gz 
[root@master download]# cd libevent-1.4.14b-stable
[root@master libevent-1.4.14b-stable]# ./configure --prefix=/usr/local/libevent 
[root@master libevent-1.4.14b-stable]# make
[root@master libevent-1.4.14b-stable]# make install
```

b. 安装memcached

```bash
[root@master download]# tar zxvf memcached-1.4.15.tar.gz 
[root@master download]# cd memcached-1.4.15
[root@master memcached-1.4.15]# ./configure --prefix=/usr/local/memcached --with-libevent=/usr/local/libevent/
[root@master memcached-1.4.15]# make
[root@master memcached-1.4.15]# make install
```

c. 启动memcached

```ba
[root@master memcached-1.4.15]# /usr/local/memcached/bin/memcached  -u root -d 
-m 2048 -l 127.0.0.1 -p 11211 -P /tmp/memcached.pid
```

+ 启动参数说明

```
-d 选项是启动一个守护进程。
-u  是运行Memcache的用户，如果当前为root 的话，需要使用此参数指定用户。
-l  是监听的服务器IP地址，默认为所有网卡。
-m 是分配给Memcache使用的内存数量，单位是MB，默认64MB。
-M return error on memory exhausted (rather than removing items)。
-p 是设置Memcache的TCP监听的端口，最好是1024以上的端口。
-c 选项是最大运行的并发连接数，默认是1024。
-P 是设置保存Memcache的pid文件。
-h 显示帮助
```

d. 关闭memcached

```
kill `cat /tmp/memcached.pid`
```

### 2 设置Memcached开机启动

只需要在`/etc/rc.d/rc.local`文件中追加下面的启动命令即可

```bash
/usr/local/memcached/bin/memcached  -u root -d 
-m 2048 -l 127.0.0.1 -p 11211 -P /tmp/memcached.pid
```



