---
layout: post
title: linux - crontab定时执行任务
category: linux
tags:
  - linux
  - centos7
---

# 配置

## 安装

- 检查本机包版本

```
rpm -q cronie
```

- 安装

```
yum install cronie
```

## centos6 操作

```
启动、停止、重启服务和重新加载配置
/sbin/service crond start
/sbin/service crond stop
/sbin/service crond restart
/sbin/service crond reload
```

## centos7 操作

```
检查服务状态
systemctl status crond.service

启动服务
systemctl start crond.service

重启服务
systemctl restart crond.service
```

## 加入系统启动

- centos7

```
systemctl enable crond.service
```

- centos6

```
vi /etc/rc.d/rc.local
/sbin/service crond start
```

## 检查服务

```
cat /etc/crontab
```

# 任务设置

## 权限

- 允许

/etc/cron.allow

- 限制

/etc/cron.deny

> 一个用户账号一行
> allow 大于 deny
> 两个文件中有一个就行

## 添加个人任务

- 任务会被写入这里

/var/spool/cron/

- 命令

crontab [-u username] [-l|-e|-r] 

> -u 指定给某个用户派任务
> -e 编辑工作内容
> -l 查询工作内容
> -r 清空任务

- 指令格式

```
* * * * * 命令
分钟 小时 时间 月份 周 指令
0-59 0-23 1-31 1-12 0-7
```

- 字符

```
*(星號)
	代表任何時刻都接受的意思！舉例來說，範例一內那個日、月、週都是 * ， 就代表著『不論何月、何日的禮拜幾的 12:00 都執行後續指令』的意思！

,(逗號)
代表分隔時段的意思。舉例來說，如果要下達的工作是 3:00 與 6:00 時，就會是：0 3,6 * * * command時間參數還是有五欄，不過第二欄是 3,6 ，代表 3 與 6 都適用！

-(減號)
代表一段時間範圍內，舉例來說， 8 點到 12 點之間的每小時的 20 分都進行一項工作：20 8-12 * * * command仔細看到第二欄變成 8-12 喔！代表 8,9,10,11,12 都適用的意思！

/n(斜線)
那個 n 代表數字，亦即是『每隔 n 單位間隔』的意思，例如每五分鐘進行一次，則：*/5 * * * * command很簡單吧！用 * 與 /5 來搭配，也可以寫成 0-59/5 ，相同意思！
```




[我的博客](https://hans007.github.io)