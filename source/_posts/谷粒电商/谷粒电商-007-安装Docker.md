---
title: 谷粒电商-007-安装Docker
date: 2020-02-15 18:09:04
tags:
    - 谷粒电商
    - Docker
---

# 在Dubbo架构中：注册中心 Registry
用Zookeeper注册中心

使用Docker安装Zookeeper。


# 搭建

## docker 安装、启动、停止
1. 查看centos版本
    docker要求centos系统的内核版本高于3.10.
    $ uname -r 可查看

2. 若版本低，升级软件包及内核；
    $ yum update

3. 安装docker
    $ yum install docker

4. 启动docker
    $ systemctl start docker
    $ docker -v  可查看docker version

5. 将docker服务设为开机启动
    $ systemctl enable docker

6. 停止
    $ systemctl stop docker
      
## docker 常用操作
1. 检索
    $ docer search xxx 

2. 拉取
    $ docker pull 镜像名:tag     :tag可选 表示标签 多为软件的版本 默认latest

3. 列表
    $ docker images         查看所有本地镜像

4. 删除
    $ docker rmi image-id    删除指定的本地镜像

可取Docker Hub官网去查看 镜像及其版本

## docker容器操作
软件镜像 --- 运行镜像 --- 产生一个容器（正在运行的软件）

1. 运行
    $ docker run --name container-name -d image-name   
    eg. $ docker run --name mytomcat -d tomcat:latest 
                --name自定义容器名 -d后台运行 image-name指定镜像模板

2. 列表
    $ docker ps  查看运行中的容器  加上-a 可以查看所有容器

3. 停止运行中的容器
    $ docker stop container-id或容器名

4. 查看所有的容器，包括运行的和退出的
    $ docker ps -a

5. 运行容器
    $ docker start container-id

6. 删除一个容器(待删除的容器必须是stop的状态)
    $ docker rm container-id

======== 做一个在外部可以访问的tomcat ================

7. 端口映射
    $ docker run --name mytomcat -d -p 8888:8080 tomcat
    $ docker run -d -p 主机端口:容器端口

8. 浏览器可以访问 http://192.168.241.130:8888/
    需要提前关闭linux防火墙。$ service firewalld status 查看linux防火墙状态
    $ service firewalld stop 关闭防火墙

9. $ docker logs 容器id或容器名称       可查看日志

10. 可以同时启动多个容器，对于一个镜像而言。
        $ docker run -d -p 8889:8080 tomcat
        $ docker run -d -p 8887:8080 tomcat


## mysql
1. 启动mysql
    错误：$ docker run --name xxx -d mysql  使用$docker ps -a 查看后发现mysql又退出了
            $ docker logs container-id  查看日志

    正确：$ docker run --name mysql01 -d -e MYSQL_ROOT_PASSWORD=123456 mysql
            但是外部client无法访问此mysql，因为没做port映射

    正确：$ docker run --name mysql02 -d -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql

2. 从docker 进入mysql：
$ docker exec -it container-id /bin/bash
$ mysql -uroot -p123456
$ exit    退出mysql
$ exit    退出docker界面下的mysql bash


解决navicat 1251问题：
> alter user 'root'@'%' identified with mysql_native_password by '123456';
> alter user 'root'@'localhost' identified with mysql_native_password by '123456';

直接连接naivcat即可。

3. 高级操作
  把主机文件夹挂载到 容器中，这样只需要在主机上的文件夹/conf/mysql内文件里配置，就可以作用到容器
  $ docker run --name xxx -v /conf/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql

  直接命令行配置
  $ docker run --name xxx -e xxx -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

## 附录
$ docker logs container-id  可查看日志
