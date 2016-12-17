# CENTOS6.5下postgresql的安装与配置

## Introduction
**PostgreSQL** 是一种非常复杂的对象-关系型数据库管理系统（ORDBMS），也是目前功能最强大，特性最丰富和最复杂的自由软件数据库系统。有些特性甚至连商业数据库都不具备。这个起源于伯克利（BSD）的数据库研究计划目前已经衍生成一项国际开发项目，并且有非常广泛的用户。

## Environment
CENTOS6.5  
[Postgresql9.4.5](http://ftp.postgresql.org/pub/source/)
## Processing

1. 检查PostgreSQL 是否已经安装
```bash
rpm -qa|grep postgres
```
+ 若已经安装，则使用rpm -e 命令卸载。
2. 解压已经下载好的Postgresql
```bash
tar xjf postgresql-9.4.5.tar.bz2
```
3. 进入解压好的目录
```bash
cd postgresql-9.4.5
```
4. 安装依赖包
```bash
yum -y install gcc*
yum -y install readline-devel
```
5. 增加用户设置密码(此处密码设置为 *123456* 所以会提示简单)  
```bash
[root@master postgresql-9.4.5]# adduser postgres
[root@master postgresql-9.4.5]# passwd postgres
Changing password for user postgres.
New password: 
BAD PASSWORD: it is too simplistic/systematic
BAD PASSWORD: is too simple
Retype new password: 
passwd: all authentication tokens updated successfully.
```
6. 开始编译安装PostgreSQL数据库
```bash
[root@master postgresql-9.4.5]# ./configure
[root@master postgresql-9.4.5]# make 
[root@master postgresql-9.4.5]# make install
```
7. 设置环境变量(进入postgres目录修改)
```bash
[postgres@master ~]$ cd /home/postgres/
[postgres@master ~]$ vim .bash_profile 
```
+ 将`PATH=$PATH:$HOME/bin`改成`PATH=$PATH:$HOME/bin:/usr/local/pgsql/bin`
8. 初始化数据库
```bash
[root@master pgsql]# cd /usr/local/pgsql/
[root@master pgsql]# mkdir data
[root@master pgsql]# chown postgres:postgres data/
[root@postgresql ~]# su - postgres
[postgres@master ~]$ /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data/
```
9. 开启postgresql数据库服务
```bash
[postgres@postgresql ~]$ exit
```
* 复制安装目录下的linux文件到/etc/init.d/
```bash
[root@postgresql postgresql-9.4.5]# cp contrib/start-scripts/linux /etc/init.d/postgresql
```
+ 启动数据库并设置开机启动
```bash
[root@master postgresql-9.4.5]#  /etc/init.d/postgresql start
Starting PostgreSQL: ok
[root@postgresql postgresql-9.4.5]# chkconfig --add postgresql
[root@postgresql postgresql-9.4.5]# chkconfig postgresql on
```
+ 创建数据库操作历史记录文件
```bash
[root@master postgresql-9.4.5]# touch /usr/local/pgsql/.pgsql_history
[root@master postgresql-9.4.5]# cd /usr/local/pgsql/
[root@master pgsql]# chown postgres:postgres .pgsql_history 
```
10. 测试是否成功
```bash
[root@postgresql postgresql-9.4.5]# su - postgres
[postgres@postgresql ~]$ createdb test
[postgres@postgresql ~]$ psql test
psql (9.4.3)
Type "help" for help.
test=#
```
完成！

参考：[http://blog.csdn.net/jxzhfei/article/details/47150301](http://blog.csdn.net/jxzhfei/article/details/47150301)
