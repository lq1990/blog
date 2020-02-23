---
title: HTTP协议
date: 2020-02-22 18:17:07
tags:
    - HTTP
---

# http协议的特点
1. 支持客户/服务器模式

2. 简单快速
   客户向server请求服务时，只需传送请求方法和路径。常用请求方法：GET, HEAD, POST

3. 灵活
   http允许传输任意类型的数据对象。类型由Content-Type来标记。

4. 无连接
   无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并受到客户的应答后，即断开连接。
   采用这种方式可以节省传输时间。

5. 无状态
   http协议是无状态协议，即指对于事务处理没有记忆能力。
   两种保持http连接状态的技术：Cookie, Session.
   - Cookie可以记录用户登录信息，或购物车信息 以方便在付款时提取信息
   - Session：通过服务器来保持状态，一个user一个Session，用SessionId区分。



# 参考
https://www.cnblogs.com/xuxinstyle/p/9813654.html
