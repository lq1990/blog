---
title: 谷粒电商-006-Dubbo简介
date: 2020-02-15 17:18:12
tags: 
    - 谷粒电商
    - Dubbo
---

# Dubbo
是高性能、轻量级的Java RPC框架。

三大核心能力：
- 面向接口的远程方法调用
- 智能容错
- 负载均衡
- 另外：服务自动注册和发现

## 架构图
![](/images/谷粒电商/Dubbo架构图.png)

1. Container是 Dubbo容器
2. Provider要注册到注册中心
3. Consumer订阅注册中心，注册中心有Provider消息时会通知Consumer
4. Consumer有了Provider了地址，就可调用了
5. Monitor在运维时可用。

## 后续
在查资料时发现，Dubbo+Zookeeper和SpringCloud两者各分秋色。
