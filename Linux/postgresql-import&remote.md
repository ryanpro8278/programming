# Postgresql数据的导入及远程访问的开启
## 介绍
主要介绍内容为：  
1、Postgresql的基本使用、数据导入(执行sql文件)
2、开启Postgresql远程访问：远程访问开启后可以通过Windows端的图形化控制工具来访问我们安装在CentOS系统下的postgresql数据库，
对于不熟悉linux系统的用户来说会比较方便浏览数据库。
## 环境
CentOS6.5  
Postgresql9.4.5
测试用的 **qz.sql** 文件
## 步骤
### ①使用及数据导入
1. 首先进入之前建立的数据库账户
```bash
[root@master local]# su - postgres
[postgres@master ~]$ 
```
2. 试用pqsql的命令
```
创建一个新的数据库： 
createdb xxxx; 
创建一个新的PostgreSQL用户： 
createuser xxxx;  
*重命名一个表： 
alter table [表名A] rename to [表名B]; 
*删除一个表： 
drop table [表名]; 
```
