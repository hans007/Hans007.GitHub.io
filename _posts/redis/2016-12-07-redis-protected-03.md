---
layout: post
title: redis - 第三节:保护模式配置
category: redis
tags:
  - redis
---

# 保护模式下非本地连接不能访问

## 问题还原

我是一台机器做redis服务，一台机器当客户端连接

> redis server : 10.211.55.8:6379

- 启动服务

```
$ redis-server ./redis.conf
```

> redis.conf是官方安装默认的文件

- 客户端

输入

```
$ redis-cli -h 10.211.55.8 -p 6379
10.211.55.8> set key:01 val123456
```

返回

```
(error) DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```

> 不带换行的错误提示，我们耐心看下，无非就是告诉我们当前是保护模式，有几个解决方案。

## 方案一：绑定服务器ip

```
$ vi ./redis.conf

......

################################## NETWORK #####################################

# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
bind 10.211.55.8
```

> 加入配置 bind 10.211.55.8

## 方案二：修改保护模式 protected-mode

```
$ vi ./redis.conf

......

# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode no
```

> 设置 protected-mode no

## 方案三：设置口令(推荐)

```
$ vi ./redis.conf

......

################################## SECURITY ###################################

# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared
requirepass 123456789
```

- 客户端

```
$ auth 123456789
OK
$ set key:01 val123456
OK
$ get key:01
"val123456"
```


[我的博客](https://hans007.github.io)