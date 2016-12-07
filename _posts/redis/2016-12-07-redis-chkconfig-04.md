---
layout: post
title: redis - 第四节:centos7开机自启动
category: redis
tags:
  - redis
---

# centos7开机自启动

## 配置 redis.conf

```
$ vi ./redis.conf

...

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
#daemonize no
daemonize yes

# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/run/redis.pid

```

> 设置守护进程运行 daemonize yes
> 设置pid文件 pidfile /var/run/redis.pid

## 编写开机自启动脚本

```
$ vi /etc/init.d/redis
```

```
#!/bin/sh
# chkconfig: 2345 10 90
# description: Start and Stop redis

PATH=/usr/local/bin:/sbin:/usr/bin:/bin
REDISPORT=6379
EXEC=/root/src/redis-3.2.5/src/redis-server
REDIS_CLI=/root/src/redis-3.2.5/src/redis-cli

PIDFILE=/var/run/redis.pid
CONF="/root/src/redis-3.2.5/redis.conf"
AUTH=""

case "$1" in
        start)
                if [ -f $PIDFILE ]
                then
                        echo "$PIDFILE exists, process is already running or crashed."
                else
                        echo "Starting Redis server..."
                        $EXEC $CONF
                fi
                if [ "$?"="0" ]
                then
                        echo "Redis is running..."
                fi
                ;;
        stop)
                if [ ! -f $PIDFILE ]
                then
                        echo "$PIDFILE exists, process is not running."
                else
                        PID=$(cat $PIDFILE)
                        echo "Stopping..."
                       $REDIS_CLI -p $REDISPORT  SHUTDOWN
                        sleep 2
                       while [ -x $PIDFILE ]
                       do
                                echo "Waiting for Redis to shutdown..."
                               sleep 1
                        done
                        echo "Redis stopped"
                fi
                ;;
        restart|force-reload)
                ${0} stop
                ${0} start
                ;;
        *)
               echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2
                exit 1
esac
```

头三行不能少

```
#!/bin/sh
# chkconfig: 2345 10 90
# description: Start and Stop redis
```

## 设置权限

```
$ chmod 755 redis
```

## 测试

- 启动

```
/etc/init.d/redis start
```

- 关闭

```
/etc/init.d/redis stop
```

## 设置开机自启动

```
$ chkconfig redis on
```

## 关机重启测试

```
$ reboot
```

## 检测后台进程是否存在

```
$ ps -ef |grep redis
```

## 检测6379端口是否在监听

```
$ netstat -lntp | grep 6379
```


[我的博客](https://hans007.github.io)