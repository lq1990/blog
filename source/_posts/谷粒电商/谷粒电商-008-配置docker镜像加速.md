---
title: 谷粒电商-008-配置docker镜像加速
date: 2020-02-15 20:16:20
tags:
    - 谷粒电商
    - Docker
---

# Zookeeper 安装
进行docker镜像加速配置：
![](/images/谷粒电商/aliyun-mirror.png)

1. 下载zookeeper 镜像
   $ docker pull zookeeper

2. 启动容器并添加映射
   $ docker run -p 2181:2181 --privileged=true --name zookeeper -d zookeeper
