---
layout: post
title: linux - centos6常用shell
category: linux
tags:
  - linux
  - centos6
---

# centos6常用shell

## 系统启动加载

vi /etc/rc.d/rc.local

## 查看端口占用

netstat -ntlp

## 查询进程

ps -ef | grep xxxx

## iptables防火墙

vi /etc/sysconfig/iptables
service iptables restart
/etc/init.d/iptables status


