---
title: linux防火墙相关命令
date: 2020-02-15 18:23:13
tags: linux
---

## 查看防火墙状态
$ systemctl status firewalld
$ firewall-cmd --state
## 停止防火墙
$ systemctl stop firewalld
## 禁止开机启动
$ systemctl disable firewalld

## 开启
$ service firewalld start
## 重启
$ service firewalld restart
## 关闭
$ service firewalld stop

## 查看防火墙规则
$ firewall-cmd--list-all

## 查询端口是否开放
firewall-cmd --query-port=8080/tcp
## 开放80端口
firewall-cmd --permanent --add-port=80/tcp
## 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
## 重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload

