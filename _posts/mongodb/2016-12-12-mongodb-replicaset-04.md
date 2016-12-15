---
layout: post
title: MongoDB - replication复制
category: MongoDB
tags:
  - linux
  - MongoDB
  - replication
---

# Replication(副本) 主备模式

## 手册

> https://docs.mongodb.com/v3.2/replication/

## hosts规划

```
$ vi /etc/hosts

127.0.0.1    mongodb-a1
127.0.0.1    mongodb-a2
127.0.0.1    mongodb-a3
```

## 目录规划

```
数据
/www/mongodb/db/a1
/www/mongodb/db/a2
/www/mongodb/db/a3

验证文件
/www/mongodb/key/mongodb-keyfile

配置文件
/www/mongodb/conf/mongod-a1.conf
/www/mongodb/conf/mongod-a2.conf
/www/mongodb/conf/mongod-a3.conf

日志文件
/www/mongodb/log/a1.log
/www/mongodb/log/a2.log
/www/mongodb/log/a3.log
```

## 配置文件

```
$ vi /www/mongodb/conf/mongod-a1.conf
$ sudo chown -R mongod:mongod /www/mongodb/db/a1

processManagement:
    fork: true
storage:
  dbPath: /www/mongodb/db/a1
  journal:
    enabled: true
systemLog:
  destination: file
  logAppend: true
  path: /www/mongodb/log/a1.log
net:
  port: 28001
  bindIp: 0.0.0.0
replication:
   replSetName: rstest
```

## 启动服务

```
$ mongod -f /www/mongodb/conf/mongod-a1.conf
$ mongod -f /www/mongodb/conf/mongod-a2.conf
$ mongod -f /www/mongodb/conf/mongod-a3.conf
```

## 配置集群

```
$ mongo mongodb-a1:28001
> use admin
> rs.initiate()
> rs.conf()
> rs.add("mongodb-a2:28002")
> rs.add("mongodb-a3:28003")
> rs.status()
```

## 修改副本名字replSetName

```
use local  
var doc = db.system.replset.findOne()  
doc._id = 'NewReplicaSetName'
db.system.replset.save(doc)  
db.system.replset.remove({_id:'OldReplicaSetName'})
```

## 修改config配置

```
cfg = rs.conf()
cfg["version"] = 2
rs.reconfig(cfg, { force: true})
```

## 测试数据

- 主服务器: 插入10000数据

```
$ mongo mongodb-a1:28001
> use test
> db.isMaster()
> for(i=0;i<10000;i++){db.userid.insert({"user_id":i})}
> db.userid.find().count()
```

- 从服务器查看

```
$ mongo --nodb

> conn = new Mongo("mongodb-a2:28002")
> db = conn.getDB("test")
> conn.setSlaveOk()
> db.userid.find().count()

> conn2 = new Mongo("mongodb-a3:28003")
> db2 = conn2.getDB("test")
> conn2.setSlaveOk()
> db2.userid.find().count()
```

## key验证文件

- 生成key文件

```
$ openssl rand -base64 741 > /www/mongodb/key/mongodb-keyfile
$ chmod 600 /www/mongodb/key/mongodb-keyfile
$ chown mongod:mongod /www/mongodb/key/mongodb-keyfile
```

- 改配置

```
$ vi /www/mongodb/conf/mongod-a1.conf

security:
  keyFile:/www/mongodb/key/mongodb-keyfile
```

## 注意说明

> 有效节点不能少于3，否者不能正常工作

## 调试备用

```
mongod --shutdown --dbpath=/www/mongodb/db/a1
ps -aux|grep mongo
netstat -lntp | grep 27017
```

[我的博客](https://hans007.github.io)