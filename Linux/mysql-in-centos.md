# CentOS6.5系统下安装Mysql5.6

## 介绍
MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，
目前属于 Oracle 旗下产品。MySQL 最流行的关系型数据库管理系统，
在 WEB 应用方面MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

## 环境
CentOS6.5  
[MySQL5.6.34-1.tar](http://mirrors.sohu.com/mysql/MySQL-5.6/MySQL-5.6.34-1.linux_glibc2.5.x86_64.rpm-bundle.tar)

## 步骤
### 1 安装mysql

a. 首先检查系统中是否已经含有关于 **mysql** 的软件

```bash
#rpm -qa | grep -i mysql  //grep -i是不分大小写字符查询，只要含有mysql就显示
```
+ 屏幕可能显示
```bash
mysql-libs-5.1.71-1.el6.x86_64
```
+ 使用下面的代码删除该libs
```bash
# yum remove mysql-libs
```

b. 解压已经下载好的mysql的tar文件

```bash
# tar xvf MySQL-5.6.34-1.linux_glibc2.5.x86_64.rpm-bundle.tar
MySQL-client-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-devel-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-embedded-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-server-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-shared-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-shared-compat-5.6.34-1.linux_glibc2.5.x86_64.rpm
MySQL-test-5.6.34-1.linux_glibc2.5.x86_64.rpm
```
c. 安装 **MySQL-shared-compat** 替换mysql-libs

```bash
# rpm -i MySQL-shared-compat-5.6.34-1.linux_glibc2.5.x86_64.rpm 
# rpm -qa | grep -i mysql
MySQL-shared-compat-5.6.34-1.linux_glibc2.5.x86_64
```
d. 安装 **perl** (MySQL-server所必须的环境)

```bash
# rpm -ivh --test MySQL-server-5.6.34-1.linux_glibc2.5.x86_64.rpm
# yum install perl
```
e. 安装 **MySQL-server** 和 **MySQL-client** ，可以看到密码被保存在 **/root/.mysql_secret**

```bash
# rpm -ivh  MySQL-server-5.6.34-1.linux_glibc2.5.x86_64.rpm
Preparing...                ########################################### [100%]
   1:MySQL-server           ########################################### [100%]
………………
………………
A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !
You will find that password in '/root/.mysql_secret'.
You must change that password on your first connect,
no other statement but 'SET PASSWORD' will be accepted.
See the manual for the semantics of the 'password expired' flag.
Also, the account for the anonymous user has been removed.
In addition, you can run:
  /usr/bin/mysql_secure_installation
………………
………………
# rpm -ivh  MySQL-client-5.6.34-1.linux_glibc2.5.x86_64.rpm
Preparing...                ########################################### [100%]
   1:MySQL-client           ########################################### [100%]
```
### 2 初始化mysql

a. 获取初始密码

```bash
# more /root/.mysql_secret
# The random password set for the root user at Tue Dce 12 22:57:46 2014 (local t
ime): Ii8Ts1lWgGlUrFx2    <– 得到root访问mysql的密码：Ii8Ts1lWgGlUrFx2
```
b. 启动mysql服务

```bash
# service mysql start
Starting MySQL... SUCCESS! 
```
c. 初始化mysql数据库

```bash
# /usr/bin/mysql_secure_installation --user=mysql
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MySQL to secure it, we'll need the current
password for the root user.  If you've just installed MySQL, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):    <–使用刚才得到的root的密码 Ii8Ts1lWgGlUrFx2
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] y   <– 是否更换root用户密码，输入y并回车，强烈建议更换
New password:      <– 设置root用户的密码
Re-enter new password:    <– 再输入一次你设置的密码
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y   <– 是否删除匿名用户,生产环境建议删除，所以输入y并回车
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

                                         <-远程登录可以方便在Windows端通过某些工具直接访问linux下的数据库
Disallow root login remotely? [Y/n] y    <–是否禁止root远程登录,根据自己的需求选择Y/n并回车
 ... Success!

By default, MySQL comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y    <– 是否删除test数据库,输入y并回车
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y   是否重新加载权限表，输入y并回车
 ... Success!




All done!  If you've completed all of the above steps, your MySQL
installation should now be secure.

Thanks for using MySQL!


Cleaning up...
```
d. 检测mysql服务是否成功启动

```bash
#  chkconfig
...
...
mysql           0:off   1:off   2:on    3:on    4:on    5:on    6:off   <-看到这个OK了
...
```
完成！

参考:[http://www.linuxidc.com/Linux/2015-01/111413.htm](http://www.linuxidc.com/Linux/2015-01/111413.htm)
