# Mysql远程访问的开启
## 介绍
安装了MySQL之后，MySQL的root用户默认是不开放远程访问权限的，只需2个步骤即可开启它
## 环境
CentOS6.5  
Mysql 5.6.34  
Navicat(可视化工具)  
## 步骤
1. 开放Linux3306端口的远程连接权限  
```Bash
[root@master data]# vim /etc/sysconfig/iptables
```
+ 在 ` -A INPUT -m state –state NEW -m tcp -p tcp –dport 20 -j ACCEPT` 下面一行添加 `-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT`  
+ 完成后保存设置并重启防火墙和数据库服务
```Bash
[root@master data]# /etc/init.d/iptables restart
iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
[root@master data]# service postgresql restart
Restarting PostgreSQL: ok
```

2. 为Mysql的root用户添加远程访问权限
```Bash
[root@master bbb]# mysql -uroot -p
Enter password: （此处输入密码登录）
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 5.6.34 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
+ 输入命令并传入自己设置的Mysql下root账户的密码，随后重启
```Bash
mysql> grant all on *.* to root@'%' identified by 'yourpassword';
Query OK, 0 rows affected (0.29 sec)

mysql> exit
Bye
[root@master bbb]# /etc/init.d/mysql restart
Shutting down MySQL.. SUCCESS! 
Starting MySQL. SUCCESS! 
```

3. 测试连接
   在windows下打开Navicat新建查询并设置，成功连接：  
   ![](http://ww3.sinaimg.cn/large/82c8e86egw1fb3fq7zhx5j20gh0ipmyw.jpg)
