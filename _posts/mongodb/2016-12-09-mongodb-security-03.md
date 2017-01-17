---
layout: post
title: MongoDB - 安全设置
category: MongoDB
tags:
  - linux
  - MongoDB
---

# 基于账号的验证登录

## 屏蔽conf的security

```
$ vi /etc/mongod.conf

# security:
#  authorization: "enabled"

$ systemctl restart mongod.service
```

## 创建超级用户

```
$ mogodb
$ use admin
$ db.createUser({ user: "root" , pwd: "123", roles: ["userAdminAnyDatabase", "dbAdminAnyDatabase", "readWriteAnyDatabase"]});
```

## 规则

- 参考

> https://docs.mongodb.com/v3.2/core/security-built-in-roles/

- 超级权限

> readWriteAnyDatabase, dbAdminAnyDatabase, userAdminAnyDatabase, clusterAdmin

- 上线只用一个超级用户肯定不合适，可以针对具体数据建用户

> 上线运行设置 readWrite
> 单库管理可以设置 dbOwner

- 权限维度

> 单库、超级用户、服务器群组、备份

## 启动授权登录

```
$ vi /etc/mongod.conf

security:
  authorization: "enabled"
```

## 验证登录

```
$ mongo
$ show dbs
2016-12-07T13:06:20.774-0500 E QUERY    [main] Error: listDatabases failed:{
	"ok" : 0,
	"errmsg" : "not authorized on admin to execute command { listDatabases: 1.0 }",
	"code" : 13,
	"codeName" : "Unauthorized"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:62:1
shellHelper.show@src/mongo/shell/utils.js:755:19
shellHelper@src/mongo/shell/utils.js:645:15
@(shellhelp2):1:1
```

> 现在默认登录 就没权限了

```
> use admin
> db.auth('root','123');
1
> show dbs
admin  0.000GB
local  0.000GB
```

> 验证成功

## 创建数据库用户

```
use mydb
db.createUser(
    {
     user: "yuu",
     pwd: "yuu:pwd",
     roles: [ { role: ["readWrite","dbOwner"], db: "mydb" } ]
   }
)
```

## 权限整理

```
内建的角色
数据库用户角色：read、readWrite;
数据库管理角色：dbAdmin、dbOwner、userAdmin；
集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
备份恢复角色：backup、restore；
所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
超级用户角色：root // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
内部角色：__system

角色说明：
Read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限
```


[我的博客](https://hans007.github.io)