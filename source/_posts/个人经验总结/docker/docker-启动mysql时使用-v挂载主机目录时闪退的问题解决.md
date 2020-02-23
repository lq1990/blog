---
title: docker-启动mysql时使用-v挂载主机目录时闪退的问题解决
date: 2020-02-20 18:17:44
tags: Docker
---

# docker-启动mysql时使用-v挂载主机目录时闪退的问题解决
1. 运行
  $ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql-master -v /home/mysql/data:/var/lib/mysql -v /home/mysql/conf:/etc/mysql mysql


大约2秒后，此mysql容器闪退。

2. 查看mysql容器日志
   $ docker logs 容器id

显示：mysqld: Error on realpath() on '/var/lib/mysql-files' (Error 2 - No such file or directory

3. 解决
另外需要指定挂载/var/lib/mysql-files目录。
  $ docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql-master -v /home/mysql/data:/var/lib/mysql -v /home/mysql/conf:/etc/mysql -v /home/mysql/mysql-files:/var/lib/mysql-files/ mysql


# 参考
https://www.cnblogs.com/s1956/p/9997691.html

# 注
如果你的mysql容器日志不是这个，那就是别的原因，可以复制你的原因百度下。
个人经验是直接百度出错原因 比使用中文描述更容易找到解决方案。