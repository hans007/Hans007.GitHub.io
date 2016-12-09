---
layout: post
title: MongoDB - 安全设置
category: MongoDB
tags:
  - linux
  - MongoDB
---

> 一个数据库没有密码总不是好事，就算网段限制，一旦被注入方式...哈哈

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



[我的博客](https://hans007.github.io)