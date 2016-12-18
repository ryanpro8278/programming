# Postgresql数据的导入及远程访问的开启
## 介绍
1、Postgresql的基本使用、数据导入(执行sql文件)  ；  
2、开启Postgresql远程访问：远程访问开启后可以通过Windows端的图形化控制工具来访问我们安装在CentOS系统下的postgresql数据库，
对于不熟悉linux系统的用户来说会比较方便浏览数据库；  
3、Postgresql的常用命令介绍
## 环境
CentOS6.5  
Postgresql9.4.5  
Navicat(可视化工具)  
测试用的 **pgads.sql** 文件
## 步骤
### ① 使用及数据导入
1. 创建数据库用户的密码(之前的 *postgres* 只是Linux下用来操作pgsql数据库的用户)
```bash
[root@master local]# su - postgres
[postgres@master pgsql]$ psql
psql (9.4.5)
Type "help" for help.

postgres=# \password
Enter new password: 
Enter it again: 
postgres=# \q
```

2. 导入现有的 **pgads.sql** 文件
```bash
[postgres@master pgsql]$ createdb pgads
[postgres@master pgsql]$ psql -U postgres -W -d pgads -f pgads.sql
Password for user postgres: 
[postgres@master pgsql]$ psql
psql (9.4.5)
Type "help" for help.

postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privile
ges   
-----------+----------+----------+-------------+-------------+-----------------
------
 pgads     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
```
导入完成！

### ② 开启远程访问
1. 修改 **\usr\local\pgsql\data\pg_hba.conf** 文件，配置用户的访问权限  
在 `IPv4 local connections:` 下添加 `host  all    all    192.168.42.1/24    md5`  
此处表示允许网段 *192.168.42.1* 上的所有主机使用所有合法的数据库用户名访问数据库，并提供加密的密码验证，**具体的网段需要根据实际情况来输入**。

2. 修改 **\usr\local\pgsql\data\postgresql.conf** 文件  
定位到 `#listen_addresses='localhost'`， PostgreSQL安装完成后，默认是只接受来在本机localhost的连接请求；
将行开头的#去掉，将行内容修改为 `listen_addresses='*'` 来允许数据库服务器监听来自任何主机的连接请求。

3. 更改防火墙设置，开放 **5432** 端口
```bash
[root@master data]# vim /etc/sysconfig/iptables
```
+ 在 `-A INPUT -m state –state NEW -m tcp -p tcp –dport 20 -j ACCEPT` 下面一行添加 `-A INPUT -m state –state NEW -m tcp -p tcp –dport 5432 -j ACCEPT`  
+ 完成后保存设置并重启防火墙和数据库服务
```bash
[root@master data]# /etc/init.d/iptables restart
iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
[root@master data]# service postgresql restart
Restarting PostgreSQL: ok
```

4. 测试连接  
在windows下打开Navicat新建查询并设置：  
![](http://ww1.sinaimg.cn/large/82c8e86egw1fav3jxb6m3j20gh0ip0ug.jpg)
+ 开启连接，成功连接到Linux下的数据库：  
![](http://ww3.sinaimg.cn/large/82c8e86egw1fav3kn6mlhj207007iaah.jpg)

## Postgresql的常用命令
1. linux下postgresql命令
```
createdb 创建一个新的PostgreSQL的数据库（和SQL语句：CREATE DATABASE 相同） 

createuser 创建一个新的PostgreSQL的用户（和SQL语句：CREATE USER 相同） 

dropdb 删除数据库 

dropuser 删除用户 

pg_dump 将PostgreSQL数据库导出到一个脚本文件 

pg_dumpall 将所有的PostgreSQL数据库导出到一个脚本文件 
```

2. postgresql控制台中的命令
```
§\q：命令退出控制台
§\password：为XXX用户设置一个密码。
§\h：查看SQL命令的解释，比如\h select。
§\?：查看psql命令列表。
§\l：列出所有数据库。
§\c [database_name]：连接其他数据库。
§\d：列出当前数据库的所有表格。
§\d [table_name]：列出某一张表格的结构。
§\du：列出所有用户。
§\e：打开文本编辑器。
§\conninfo：列出当前数据库和连接的信息。
```

参考:  
[http://blog.csdn.net/yuxuan_08/article/details/51428688](http://blog.csdn.net/yuxuan_08/article/details/51428688)  
[http://blog.csdn.net/ll136078/article/details/12747403](http://blog.csdn.net/ll136078/article/details/12747403)  
[http://www.jb51.net/LINUXjishu/63566.html](http://www.jb51.net/LINUXjishu/63566.html)  
[http://www.cnblogs.com/eastday/archive/2013/05/31/3109804.html](http://www.cnblogs.com/eastday/archive/2013/05/31/3109804.html)