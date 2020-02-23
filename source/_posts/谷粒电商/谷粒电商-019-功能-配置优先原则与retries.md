---
title: 谷粒电商-019-功能-配置优先原则与retries
date: 2020-02-17 21:10:24
tags:
    - 谷粒电商
    - Dubbo
---

# 集群容错
在集群调用失败时，Dubbo提供了多种容错方案，缺省为failover重试。

## 集群容错模式
### Failover Cluster (默认)
**失败自动切换**，当出现失败，重试其它服务器。
重试会带来更长延迟。

### Failfast Cluster
快速失败，只发起一次调用，失败立即报错。通常用于非幂等性的写操作，比如新增记录。

注：
幂等性：用户对于同一操作发起的一次请求或者多次请求的结果是一致的，不会因为多次点击而产生副作用。

### Failsafe Cluster
失败安全，出现异常时，**直接忽略**。通常用于写入审计日志等操作。

### Failback Cluster
失败自动恢复，后台记录失败请求，定时重发。通常用于消息通知操作。

### Forking Cluster
并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。可通过 forks="2" 来设置最大并行数。

### Broadcast Cluster
广播调用所有提供者，逐个调用，任意一台报错则报错。通常用于**通知所有提供者**更新缓存或日志等本地资源信息。


# XML配置：不同粒度配置的覆盖关系
对于timeout, retries, loadbalance, actives等
- 方法级优先，接口级次之，全局配置再次之
- 如果级别一样，则消费方优先，提供方次之
  
![](/images/谷粒电商/dubbo-config-priority.png)

总结: 越精确越优先



# 参考
http://dubbo.apache.org/zh-cn/docs/user/demos/fault-tolerent-strategy.html
http://dubbo.apache.org/zh-cn/docs/user/configuration/xml.html

