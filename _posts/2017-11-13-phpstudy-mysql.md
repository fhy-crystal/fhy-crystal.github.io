---
layout: post
title: phpstudy 中 mysql 的使用
excerpt: windows 下使用 phpstudy 中集成的 mysql 创建简单的数据库
categories: [教程, phpstudy, mysql]
tags: [教程, phpstudy, mysql]
modified: 2017-11-13
comments: true
---

## 前期准备
安装phpstudy，[下载地址](https://www.baidu.com/link?url=yRcr1XyRiVvvVrxIGwUYj2-EAH2Bq0uXQvDpAeJpK8BTMCWyHyIC0pm5gAy6lHOWC4jASLGaOwTtcZa78XITbc9iC4tSLXaJQufyzZ_Bw37&wd=&eqid=beac4e620000592f0000000559edbd24)

## 快速创建数据库
![phpstudyMysql](http://oy41mkgad.bkt.clouddn.com/phpstudyMysql.png 'phpstudyMysql')

其他选项 > MySql工具 > 快速创建数据库 

![createdb](http://oy41mkgad.bkt.clouddn.com/createdb.png 'createdb')

输入数据库名称`MySqlDB`

![createdDB](http://oy41mkgad.bkt.clouddn.com/createdDB.png 'createdDB')

然后一个名为`MySqlDB`的数据库就在本地创建好了

## 查看数据库
查看数据库在本地的位置：其他选项 > MySql工具 > 打开数据库目录

## 操作数据库
其他选项 > MySql工具 > MySQL命令行

打开命令行对数据库进行增删改查的简单操作

![mysqlpwd](http://oy41mkgad.bkt.clouddn.com/mysqlpwd.png 'mysqlpwd')

如果没有重新设置过密码，那么默认的都是`root`，输入密码回车之后我们就能输入sql语句，对数据库进行操作啦

## 常用sql语句
###### 显示现有数据库

```
show databases; // 必须要分号！
```

![showdatabases](http://oy41mkgad.bkt.clouddn.com/showdatabases.png 'showdatabases')

我们能看到刚刚进行快速创建的`MySqlDB`的数据库，当然，通过下面的语句我们也能够创建一个新的数据库
###### 创建数据库

```
create database testDatabase;
```
###### 选择数据库

```
use testDatabase;
```

进入`MySqlDB`数据库，在命令行输入 `use MySqlDB;`

###### 查询当前数据库基本信息

```
status;
```
![mysqlstatus](http://oy41mkgad.bkt.clouddn.com/mysqlstatus.png 'mysqlstatus')

###### 查询当前数据库

```
select database();
```
![selectdatabase](http://oy41mkgad.bkt.clouddn.com/selectdatabase.png 'selectdatabase')

###### 删除当前数据库

```
drop database testDatabase;
```

###### 显示当前数据库中的表

```
show tables;
```

###### 插入表格

```
create table person (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, name CHAR(25), age INT(9));
```
 这将创建名为"person"且包括以下三个域的数据表：id，name和age。

`INT` 命令将使得id域只能保存数字(整数)。

`NOT NULL` 命令保证id域不能为空。

`PRIMARY KEY` 则指定id域作为数据表的键域。作为键域的域不能包含重复的数据。

`AUTO_INCREMENT` 命令将自动分配递增的值到id域，尤其是将自动分配数字到对应域中。

`CHAR (字符)`和`INT (整数)`命令指定相关域中可存储的数据类型。命令旁的数字则指定对应域中可以包括多少字符或多大的整数。

![insert&show](http://oy41mkgad.bkt.clouddn.com/insert&show.png 'insert&show')

###### 删除表

```
drop table person;
```

###### 查询数据

```
select * from person;
```

###### 插入数据
```
insert into person (id, name, age) values (null, 'Crystal', 18);
```


###### 修改数据

```
update person set age=15 where id=1;
```

###### 删除数据

```
delete from person where id=4;
```

###### 删除所有数据

```
truncate person
```
![somesql](http://oy41mkgad.bkt.clouddn.com/somesql.png 'somesql')
