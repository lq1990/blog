---
title: 谷粒电商-013-dubbo-消费者调用测试
date: 2020-02-16 18:16:34
tags:
    - 谷粒电商
    - Dubbo
---

# 消费者调用测试

resources/consumer.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://code.alibabatech.com/schema/dubbo
       http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--0. 给当前应用起个名字-->
    <dubbo:application name="dubbo-user-consumer" />

    <!--1. 注册中心地址-->
    <dubbo:registry address="zookeeper://39.105.96.93:2181" />

    <!--2. 要调用哪个远程服务【服务引用】-->
    <dubbo:reference id="movieService"
                     interface="cn.wendaocp.dubbo.service.MovieService"/>

    <!--3. 将消费者加入到容器中-->
    <bean id="userService"
          class="cn.wendaocp.dubbo.service.impl.UserServiceImpl">
        <property name="movieService" ref="movieService" >
        </property>
    </bean>
</beans>
```
上面的，movieService在项目中是代理对象。
即consumer拿到 provider提供的movieService。

![](/images/谷粒电商/dubbo20880-proxy.png)

### 注意，序列化javabean
比如：
```
public class Movie implements Serializable {
...
```
### 启动顺序
1. 启动provider
2. 启动consumer

若用断点调试，发现 UserServiceImpl中的movieService是代理对象。


### 查看zktools
![](/images/谷粒电商/013-zktools.png)
