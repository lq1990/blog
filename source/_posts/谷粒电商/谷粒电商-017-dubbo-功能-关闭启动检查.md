---
title: 谷粒电商-017-dubbo-功能-关闭启动检查
date: 2020-02-17 17:41:06
tags: 
    - 谷粒电商
    - Dubbo
---

# dubbo的关闭启动检查
因为当provider未上线，而consumer上线时会报错。

consumer/resources/application.properties
```
dubbo.application.name=dubbo-user-consumer-boot
dubbo.registry.address=zookeeper://39.105.96.93:2181
server.port=8081
dubbo.consumer.check=false
```

这样，可以先启动consumer，再启动provider。
