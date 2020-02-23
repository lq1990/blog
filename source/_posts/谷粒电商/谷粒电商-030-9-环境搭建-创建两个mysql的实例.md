---
title: 谷粒电商-030-9-环境搭建-创建两个mysql的实例
date: 2020-02-19 18:24:07
tags: 
    - 谷粒电商
    - Docker
    - MySQL
---

# MySQL主从同步集群搭建

## 创建Master实例并启动
    $ docker run -p 3306:3306 --name mysql-master \
    -v /mydata/mysql/master/log:/var/log/mysql \
    -v /mydata/mysql/master/data:/var/lib/mysql \
    -v /mydata/mysql/master/conf:/etc/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -d mysql:5.7


参数说明：
  -p 3306:3306  主机3306:容器3306
  -v /mydata/mysql/master/log:/var/log/mysql 将配置文件夹挂载到主机，即：将docker容器的/var/log/mysql快捷方式到 linux的/mydata/mysql/master/log
  -v /mydata/mysql/master/data:/var/lib/mysql 将日志文件夹挂载到主机
  -v /mydata/mysql/master/conf:/etc/mysql 将配置文件夹挂载到主机
  -e MYSQL_ROOT_PASSWORD=123456 修改实例参数，初始化root的密码
  -d 后台运行

### 问题：
  $ docker logs container-id   查看闪退的mysql-m日志  
mysqld: Error on realpath() on '/var/lib/mysql-files' (Error 2 - No such file or directory

### 当前解决：
当指定了外部配置文件与外部存储路径时，也需要指定 /var/lib/mysql-files的外部目录
  $ docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql-master -v /home/mysql/master/data:/var/lib/mysql -v /home/mysql/master/conf:/etc/mysql -v /home/mysql/master/mysql-files:/var/lib/mysql-files/    mysql

参考：
https://www.cnblogs.com/s1956/p/9997691.html

### 配置mysql-master 配置文件/home/mysql/conf/my.cnf
```
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
init_connect='SET collation_connection=utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve

```

## 创建Slave实例并启动
  $ docker run -p 3316:3306 --name mysql-slaver-01 -v /home/mysql/slaver/data:/var/lib/mysql -v /home/mysql/slaver/conf:/etc/mysql -v /home/mysql/slaver/mysql-files:/var/lib/mysql-files/ -e MYSQL_ROOT_PASSWORD=123456 -d mysql


### 同样配置slave，同master配置
