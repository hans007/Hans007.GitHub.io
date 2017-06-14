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

## 开机启动

```
chkconfig crond on
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
*(星号)
	代表任何时刻都接受的意思！举例来说，范例一内那个日、月、週都是 * ， 就代表著『不论何月、何日的礼拜几的 12:00 都执行后续指令』的意思！

,(逗号)
代表分隔时段的意思。举例来说，如果要下达的工作是 3:00 与 6:00 时，就会是：0 3,6 * * * command时间参数还是有五栏，不过第二栏是 3,6 ，代表 3 与 6 都适用！

-(减号)
代表一段时间范围内，举例来说， 8 点到 12 点之间的每小时的 20 分都进行一项工作：20 8-12 * * * command仔细看到第二栏变成 8-12 喔！代表 8,9,10,11,12 都适用的意思！

/n(斜线)
那个 n 代表数字，亦即是『每隔 n 单位间隔』的意思，例如每五分钟进行一次，则：*/5 * * * * command很简单吧！用 * 与 /5 来搭配，也可以写成 0-59/5 ，相同意思！
```




[我的博客](https://hans007.github.io)