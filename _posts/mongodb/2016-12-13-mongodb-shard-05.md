---
layout: post
title: MongoDB - sharding分片
category: MongoDB
tags:
  - linux
  - MongoDB
  - sharding
---

# sharding 分片

## 手册

> https://docs.mongodb.com/v3.2/sharding/
> https://docs.mongodb.com/v3.2/reference/configuration-options/#mongos-only-options

## hosts规划

```
$ vi /etc/hosts

127.0.0.1    mongodb-a1
127.0.0.1    mongodb-a2
127.0.0.1    mongodb-a3

127.0.0.1    mongodb-c1
127.0.0.1    mongodb-c2
127.0.0.1    mongodb-c3

127.0.0.1    mongodb-s
```

## 目录规划

```
数据
/www/mongodb/db/a1
/www/mongodb/db/a2
/www/mongodb/db/a3
/www/mongodb/db/c1
/www/mongodb/db/c2
/www/mongodb/db/c3
/www/mongodb/db/s

验证文件
/www/mongodb/key/mongodb-keyfile.rsa

配置文件
/www/mongodb/conf/mongod-a1.conf
/www/mongodb/conf/mongod-a2.conf
/www/mongodb/conf/mongod-a3.conf
/www/mongodb/conf/mongod-c1.conf
/www/mongodb/conf/mongod-c2.conf
/www/mongodb/conf/mongod-c3.conf
/www/mongodb/conf/mongod-s.conf

日志文件
/www/mongodb/log/a1.log
/www/mongodb/log/a2.log
/www/mongodb/log/a3.log
/www/mongodb/log/c1.log
/www/mongodb/log/c2.log
/www/mongodb/log/c3.log
/www/mongodb/log/s.log
```

# 分片服务器

延续之前的副本集群配置，加入config服务集群，加入mongos路由服务器

##  shardsvr 

- 配置

```
$ vi /www/mongodb/conf/mongod-a1.conf
$ sudo chown -R mongodb:mongodb /www/mongodb/db/a1

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
sharding:
   clusterRole: shardsvr
net:
  port: 28001
  bindIp: 0.0.0.0
```

- 运行

```
$ mongod -f /www/mongodb/conf/mongod-a1.conf
$ mongod -f /www/mongodb/conf/mongod-a2.conf
$ mongod -f /www/mongodb/conf/mongod-a3.conf
```

## configsvr

- 配置

```
vi /www/mongodb/conf/mongod-c1.conf

storage:
  dbPath: /www/mongodb/db/c1
  journal:
    enabled: true 
systemLog:
  destination: file
  logAppend: true
  path: /www/mongodb/log/c1.log
sharding:
   clusterRole: configsvr
net:
  port: 29001
  bindIp: 0.0.0.0
processManagement:
  fork: true
replication:
   replSetName: rscfsvr
```

- 运行

```
mongod -f /www/mongodb/conf/mongod-c1.conf

$ mongo mongodb-c1:29001
> use admin
> rs.initiate()
> rs.conf()
> rs.add("mongodb-c2:29002")
> rs.add("mongodb-c3:29003")
> rs.status()
```

## 配置 MongoDB Router

```
vi /www/mongodb/conf/mongod-s.conf

systemLog:
  destination: file
  logAppend: true
  path: /www/mongodb/log/s.log
 
sharding:
  configDB: rscfsvr/localhost.localdomain:29001,mongodb-c2:29002,mongodb-c3:29003

net:
  port: 30001
  bindIp: 0.0.0.0
 
processManagement:
  fork: true
```

## 运行 MongoDB Router

```
$ mongos -f /www/mongodb/conf/mongod-s.conf

$ ps -aux|grep mongo
root      2310  0.7  3.4 763972 64628 ?        Sl   00:34   0:40 mongod -f /www/mongodb/conf/mongod-a2.conf
root      2335  0.7  3.2 754192 61876 ?        Sl   00:34   0:40 mongod -f /www/mongodb/conf/mongod-a3.conf
root      2446  0.7  3.4 836340 65488 ?        Sl   00:54   0:33 mongod -f /www/mongodb/conf/mongod-a1.conf
root      3057  0.8  3.1 892784 59564 ?        Sl   02:02   0:01 mongod -f /www/mongodb/conf/mongod-c1.conf
root      3090  0.8  2.8 840808 53976 ?        Sl   02:02   0:01 mongod -f /www/mongodb/conf/mongod-c2.conf
root      3123  0.8  3.2 811844 60432 ?        Sl   02:02   0:01 mongod -f /www/mongodb/conf/mongod-c3.conf
root      3364  0.7  0.6 219100 11592 ?        Sl   02:05   0:00 mongos -f /www/mongodb/conf/mongod-s.conf

$ netstat -lntp
tcp        0      0 0.0.0.0:29001           0.0.0.0:*               LISTEN      3057/mongod
tcp        0      0 0.0.0.0:29002           0.0.0.0:*               LISTEN      3090/mongod 
tcp        0      0 0.0.0.0:29003           0.0.0.0:*               LISTEN      3123/mongod 
tcp        0      0 0.0.0.0:28001           0.0.0.0:*               LISTEN      2446/mongod 
tcp        0      0 0.0.0.0:28002           0.0.0.0:*               LISTEN      2310/mongod 
tcp        0      0 0.0.0.0:28003           0.0.0.0:*               LISTEN      2335/mongod
tcp        0      0 0.0.0.0:30001           0.0.0.0:*               LISTEN      3364/mongos
```

## 配置分片

- 第一步: 连接到mongos

```
$ mongo mongodb-s:30001
```

- 第二步: 添加分片 Add Shards

操作

```
> use admin
> sh.addShard("mongodb-a1:28001")
> sh.addShard("mongodb-a2:28002")
> sh.addShard("mongodb-a3:28003")
```

命令说明

```
1. 单个数据库实例
{ addShard:"<hostname><:port>", maxSize:<size>, name:"<shard_name>" }

2. 副本集群
{ addShard:"<replica_set>/<hostname><:port>", maxSize:<size>, name:"<shard_name>" }

3. 如果你的momgos和 shard 在同一台机器上，添加分片不能使用 localhost 建议使用ip
```

- 第三步: Enable Sharding

```
mongos> use admin
mongos> db.runCommand({enablesharding:"shtest"})
```

- 第四步: 对一个集合进行分片

创建索引

```
> use shtest
> db.userid.ensureIndex({"user_id":1})
```

按索引创建分块

```
mongos> db.runCommand({"shardcollection":"shtest.userid",key:{"user_id":1}})
```

命令说明

```
1. db.runCommand({shardcollection:"<namespace>", key:"<key>"})
   1 表示升序

2. unique:"true/false"
启动对shard key 的唯一性约束

3. shard key 选择
```

## 分片测试

```
> use shtest

1. 查看集合状态
> db.userid.stats()

2. 查看分片状态
> db.printShardingStatus()

3. 写入数据测试
for(i=0;i<100000;i++){db.userid.insert({"user_id":i})}
```

## 列表shard

```
> use admin
> db.runCommand({listshards:1})
```

## 查看shard

```
> printShardingStatus()
```

## 是否sharding

```
db.runCommand({isdbgrid:1})
```

## 查看状态

```
use shtest
db.userid.stats()
```

## 修改shard

```
use config
db.shards.find()
db.shards.remote()
```

## 删除shard

```
db.runCommand({removeshard:”127.0.0.1:6666”}). 
```

## 调试备用

```
mongod --shutdown --dbpath=/www/mongodb/db/a1
mongod --restart --dbpath=/www/mongodb/db/a1
ps -aux|grep mongo
netstat -lntp | grep 27017

chown mongodb:mongodb /www/mongodb/key/mongodb-keyfile
chown root:root /www/mongodb/key/mongodb-keyfile

vi /www/mongodb/conf/mongod-a1.conf
mongod --shutdown --dbpath=/www/mongodb/db/a1
mongod -f /www/mongodb/conf/mongod-a1.conf
mongo mongodb-a1

```

[我的博客](https://hans007.github.io)