---
title: 谷粒电商-012-dubbo-提供者暴露
date: 2020-02-16 13:18:19
tags: 
    - 谷粒电商
    - Dubbo
---

# Provider暴露
用Spring配置声明暴露服务。

由Dubbo架构图可知，需要注册中心，使用**zookeeper注册中心**，确保server的zookeeper在运行。

## 编写Dubbo的Consumer, Provider配置，使他们能远程通信
使用dubbo，三步走
1. 在父工程**dubbo-api**中，引入dubbo以及操作Zookeeper的框架
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.2</version>
</dependency>

<!-- dubbo 2.6以前版本使用 zkclient操作zookeeper，
     dubbo 2.6以后版本使用 curator 操作zookeeper -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.12.0</version>
</dependency>
```

2. 配置provider，将服务暴露。即将provider注册进注册中心，使别人知道
resources/provider.xml 在provider项目里
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://code.alibabatech.com/schema/dubbo
       http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--配置暴露服务-->

    <!--1. 为当前应用起一个名字-->
    <dubbo:application name="dubbo-movie-provider"/>

    <!--2. 告诉dubbo注册中心的地址-->
    <dubbo:registry address="zookeeper://39.105.96.93:2181" />

    <!--3. 用dubbo协议在20880端口暴露服务。这样，消费者就会与本机的20880建立连接进行通信-->
    <dubbo:protocol name="dubbo" port="20880" />

    <!--4. 声明需要暴露的服务接口。Provider有很多接口，但需要对外暴露的只有个别。ref指向实现类的bean名字-->
    <dubbo:service interface="cn.wendaocp.dubbo.service.MovieService"
                   ref="movieService" />

    <!--5. 有了接口暴露，还需要 和本地bean一样实现服务-->
    <bean id="movieService"
          class="cn.wendaocp.dubbo.service.impl.MovieServiceImpl" />
</beans>
```

3. 在provider项目中，新建MainApp 测试下读取provider.xml
```
public class MainApplication {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext ctx =
                new ClassPathXmlApplicationContext("provider.xml");

        System.out.println("容器启动。。。");

        ctx.start();
        System.in.read();
    }
}
```
有输出，则xml可读取。

另外：通过zookeeper可视化工具zktools可查看到更新。
   
# 参考资料
https://www.jianshu.com/p/d05a3eff02f1
https://www.cnblogs.com/jpfss/p/11510873.html

