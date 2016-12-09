---
layout: post
title: MongoDB - mongod.conf重要配置
category: MongoDB
tags:
  - linux
  - MongoDB
---

# mongod.conf

```
$ vi /etc/mongod.conf
```

# 手册

> https://docs.mongodb.com/manual/reference/configuration-options
> https://docs.mongodb.com/manual/reference/parameters/

# 进程管理

```
processManagement:
  fork: true  # fork and run in background
  pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
```

名称|说明
-----------|---------------
fork       | 运行在后台
pidFilePath| PID文件路径

# 网络

```
net:
  port: 27017
  bindIp: 127.0.0.1  # Listen to local interface only, comment to listen on all interfaces.
```

名称|说明
-----------------------|---------------------------------------
port                   | 端口
bindIp                 | 绑定外网op 多个用逗号分隔
maxIncomingConnections | 进程允许的最大连接数 默认值为65536
wireObjectCheck        | 当客户端写入数据时 检测数据的有效性(BSON) 默认值为true
ipv6                   | 默认值为false

# 存储

```
storage:
  dbPath: /var/lib/mongo
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:
```

名称|说明
-----------------------|---------------------------------------
dbPath                 | mongod进程存储数据目录，此配置仅对mongod进程有效
indexBuildRetry        | 当构建索引时mongod意外关闭，那么再次启动是否重新构建索引；索引构建失败，mongod重启后将会删除尚未完成的索引，但是否重建由此参数决定。默认值为true。
repairPath             | 配合--repair启动命令参数，在repair期间使用此目录存储临时数据，repair结束后此目录下数据将被删除，此配置仅对mongod进程有效。不建议在配置文件中配置，而是使用mongod启动命令指定。
engine                 | 存储引擎类型，mongodb 3.0之后支持“mmapv1”、“wiredTiger”两种引擎，默认值为“mmapv1”；官方宣称wiredTiger引擎更加优秀。
journal                | 是否开启journal日志持久存储，journal日志用来数据恢复，是mongod最基础的特性，通常用于故障恢复。64位系统默认为true，32位默认为false，建议开启，仅对mongod进程有效。
directoryPerDB         | 是否将不同DB的数据存储在不同的目录中 默认值为false
syncPeriodSecs         | mongod使用fsync操作将数据flush到磁盘的时间间隔，默认值为60（单位：秒）强烈建议不要修改此值 mongod将变更的数据写入journal后再写入内存，并间歇性的将内存数据flush到磁盘中，即延迟写入磁盘，有效提升磁盘效率
mmapv1                 | 仅对MMAPV1引擎
  quota:               |
    enforced:false     | 配额管理，是否限制每个DB所能持有的最大文件数量 默认值为false
    maxFilesPerDB:8    | 如果enforce开启，每个DB所持有的存储文件不会超过此阀值
smallFiles: false      | 是否使用小文件存储数据；如果此值为true mongod将会限定每个数据文件的大小为512M（默认最大为2G），journal降低到128M（默认为1G）。如果DB的数据量较大，将会导致每个DB创建大量的小文件，这对性能有一定的影响。在production环境下，不建议修改此值，在测试时可以设置为true，节约磁盘。
journal:               | 
  commitIntervalMs: 100 | mongod进程提交journal日志的时间间隔，即fsync的间隔。单位：毫秒
nsSize:                | 每个database的namespace文件的大小，默认为16，单位：M；最大值可以设置为2048，即dbpath下“.ns”后缀文件的大小。16M基本上可以保存24000条命名条目，新建一个collection或者index信息，即会增加一个namespace条目；如果你的database下需要创建大量的collection（比如数据分析），则可以适度调大此值。
wiredTiger             | 如下配置仅对wiredTiger引擎生效（3.0以上版本）
  engineConfig:        |
    cacheSizeGB: 8     | wiredTiger缓存工作集（working set）数据的内存大小，单位：GB，此值决定了wiredTiger与mmapv1的内存模型不同，它可以限制mongod对内存的使用量，而mmapv1则不能（依赖于系统级的mmap）。默认情况下，cacheSizeGB的值为假定当前节点只部署一个mongod实例，此值的大小为物理内存的一半；如果当前节点部署了多个mongod进程，那么需要合理配置此值。如果mongod部署在虚拟容器中（比如，lxc，cgroups，Docker）等，它将不能使用整个系统的物理内存，则需要适当调整此值。默认值为物理内存的一半。
journalCompressor: snappy | journal日志的压缩算法，可选值为“none”、“snappy”、“zlib”。
directoryForIndexes: false| 是否将索引和collections数据分别存储在dbPath单独的目录中。即index数据保存“index”子目录，collections数据保存在“collection”子目录。默认值为false，仅对mongod有效。
collectionConfig:      |
  blockCompressor: snappy | collection数据压缩算法，可选值“none”、“snappy”、“zlib”。开发者在创建collection时可以指定值，以覆盖此配置项。如果mongod中已经存在数据，修改此值不会带来问题，旧数据仍然使用原来的算法解压，新数据文件将会采用新的解压缩算法。
indexConfig:           | 
  prefixCompression: true | 是否对索引数据使用“前缀压缩”（prefix compression，一种算法）。前缀压缩，对那些经过排序的值存储，有很大帮助，可以有效的减少索引数据的内存使用量。默认值为true。

# 性能分析器

```
operationProfiling:
```

名称|说明
-----------------------|---------------------------------------
slowOpThresholdMs: 100 | 数据库profiler判定一个操作是“慢查询”的时间阀值，单位毫秒；mongod将会把慢查询记录到日志中，即使profiler被关闭。当profiler开启时，慢查询记录还会被写入“system.profile”这个系统级的collection中。请参看mongod profiler相关文档。默认值为100，此值只对mongod进程有效。
mode: off              | 数据库profiler级别，操作的性能信息将会被写入日志文件中，可选值：
                       | 1）off：关闭profiling
                       | 2）slowOp：on，只包含慢操作日志
                       | 3）all：on，记录所有操作
                       | 数据库profiling会影响性能，建议只在性能调试阶段开启。此参数仅对mongod有效。

# 主从复制

```
replication:
```

名称|说明
-----------------------|---------------------------------------
oplogSizeMB: 10240               | replication操作日志的最大尺寸，单位：MB。mongod进程根据磁盘最大可用空间来创建oplog，比如64位系统，oplog为磁盘可用空间的5%，一旦mongod创建了oplog文件，此后再次修改oplogSizeMB将不会生效。此值不要设置的太小， 应该足以保存24小时的操作日志，以保证secondary有充足的维护时间；如果太小，secondary将不能通过oplog来同步数据，只能全量同步。
enableMajorityReadConcern: false | 是否开启readConcern的级别为“majority”，默认为false；只有开启此选项，才能在read操作中使用“majority”。（3.2+版本）
replSetName: <无默认值>           | “复制集”的名称，复制集中的所有mongd实例都必须有相同的名字，sharding分布式下，不同的sharding应该使用不同的replSetName
secondaryIndexPrefetch: all      | 只对mmapv1存储引擎有效。复制集中的secondary，从oplog中运用变更操作之前，将会先把索引加载到内存中，默认情况下，secondaries首先将操作相关的索引加载到内存，然后再根据oplog应用操作。可选值：
                                 | 1）none：secondaries不将索引数据加载到内容
                                 | 2）all：sencondaries将此操作有关的索引数据加载到内存
                                 | 3）_id_only：只加载_id索引
                                 | 默认值为：all，此配置仅对mongod有效。
localPingThresholdMs: 15         | ping时间，单位：毫秒，mongos用来判定将客户端read请求发给哪个secondary。仅对mongos有效。默认值为15，和客户端driver中的默认值一样。当mongos接收到客户端read请求，它将：
                                 | 1、找出复制集中ping值最小的member。
                                 | 2、将延迟值被此值允许的members，构建一个列表
                                 | 3、从列表中随机选择一个member。
                                 | ping值是动态值，每10秒计算一次。mongos将客户端请求转发给延迟较小（与此值相比）的某个secondary节点。

# sharding架构

```
sharding:
```

名称|说明
-----------------------|---------------------------------------
clusterRole: <无默认值>  | 在sharding集群中，此mongod实例的角色，可选值：
                        | 1、configsvr：此实例为config server，此实例默认侦听27019端口
                        | 2、shardsvr：此实例为shard（分片），侦听27018端口
                        | 此配置仅对mongod有效。通常config server和sharding server需要使用各自的配置文件。
archiveMovedChunks: true| 当chunks因为“负载平衡”而迁移到其他节点时，mongod是否将这些chunks归档，并保存在dbPath下“moveChunk”目录下，mongod不会删除moveChunk下的文件。默认为true。
autoSplit: true         | 是否开启sharded collections的自动分裂，仅对mongos有效。如果所有的mongos都设定为false，那么collections数据增长但不能分裂成新的chunks。因为集群中任何一个mongos进程都可以触发split，所以此值需要在所有mongos行保持一致。仅对mongos有效。
configDB: <无默认值>     | 设定config server的地址列表，每个server地址之间以“,”分割，通常sharded集群中指定1或者3个config server。（生产环境，通常是3个config server，但1个也是可以的）。所有的mongos实例必须配置一样，否则可能带来不必要的问题。
chunkSize: 64           | sharded集群中每个chunk的大小，单位：MB，默认为64，此值对于绝大多数应用而言都是比较理想的。chunkSize太大会导致分布不均，太小会导致分裂成大量的chunk而经常移动. 整个sharding集群中，此值需要保持一致，集群启动后修改此值将不再生效。

# 系统日志

```
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
```

名称|说明
-----------------------|---------------------------------------
verbosity: 0           | 日志级别，0：默认值，包含“info”信息，1~5，即大于0的值均会包含debug信息
quiet: true            | "安静"，此时mongod/mongos将会尝试减少日志的输出量。不建议在production环境下开启，否则将会导致跟踪错误比较困难。
traceAllExceptions: true | 打印异常详细信息。
path:  logs/mongod.log | 日志路径
logAppend: false       | 如果为true，当mongod/mongos重启后，将在现有日志的尾部继续添加日志。否则，将会备份当前日志文件，然后创建一个新的日志文件；默认为false。
logRotate: rename      | 日志“回转”，防止一个日志文件特别大，则使用logRotate指令将文件“回转”，可选值：
                       | 1）rename：重命名日志文件，默认值。
                       | 2）reopen：使用linux日志rotate特性，关闭并重新打开此日志文件，可以避免日志丢失，但是logAppend必须为true。
destination: file      | 日志输出目的地，可以指定为“ file”或者“syslog”，表述输出到日志文件，如果不指定，则会输出到标准输出中（standard output）

# 与安全有关的配置

```
security:  
    authorization: enabled  
    clusterAuthMode: keyFile  
    keyFile: /srv/mongodb/keyfile  
    javascriptEnabled: true  
setParameter:   
    enableLocalhostAuthBypass: true  
    authenticationMechanisms: SCRAM-SHA-1  
```

名称|说明
-----------------------|---------------------------------------
  authorization        | disabled或者enabled，仅对mongod有效；表示是否开启用户访问控制（Access Control），即客户端可以通过用户名和密码认证的方式访问系统的数据，默认为“disabled”，即客户端不需要密码即可访问数据库数据。（限定客户端与mongod、mongos的认证）
  clusterAuthMode      | 集群中members之间的认证模式，可选值为“keyFile”、“sendKeyFile”、“sendX509”、“x509”，对mongod/mongos有效；默认值为“keyFile”，mongodb官方推荐使用x509，不过我个人觉得还是keyFile比较易于学习和使用。不过3.0版本中，mongodb增加了对TLS/SSL的支持，如果可以的话，建议使用SSL相关的配置来认证集群的member，此文将不再介绍。（限定集群中members之间的认证）
  keyFile              | 当clusterAuthMode为“keyFile”时，此参数指定keyfile的位置，mongodb需要有访问此文件的权限。
  javascriptEnabled    | true或者false，默认为true，仅对mongod有效；表示是否关闭server端的javascript功能，就是是否允许mongod上执行javascript脚本，如果为false，那么mapreduce、group命令等将无法使用，因为它们需要在mongod上执行javascript脚本方法。如果你的应用中没有mapreduce等操作的需求，为了安全起见，可以关闭javascript。
setParameter           | 允许指定一些的Server端参数，这些参数不依赖于存储引擎和交互机制，只是微调系统的运行状态，比如“认证机制”、“线程池参数”等。参见【parameter】
  enableLocalhostAuthBypass | true或者false，默认为true，对mongod/mongos有效；表示是否开启“localhost exception”，对于sharding cluster而言，我们倾向于在mongos上开启，在shard节点的mongod上关闭。
  authenticationMechanisms  | 认证机制，可选值为“SCRAM-SHA-1”、“MONGODB-CR”、“PLAN”等，建议为“SCRAM-SHA-1”，对mongod/mongos有效；一旦选定了认证机制，客户端访问databases时需要与其匹配才能有效。

# 性能有关的参数

```
setParameter:  
    connPoolMaxShardedConnsPerHost: 200  
    connPoolMaxConnsPerHost: 200  
    notablescan: 0 
```

名称|说明
-----------------------|---------------------------------------
connPoolMaxShardedConnsPerHost   | 默认值为200，对mongod/mongos有效；表示当前mongos或者shard与集群中其他shards链接的链接池的最大容量，此值我们通常不会调整。连接池的容量不会阻止创建新的链接，但是从连接池中获取链接的个数不会超过此值。维护连接池需要一定的开支，保持一个链接也需要占用一定的系统资源。
connPoolMaxConnsPerHost          | 默认值为200，对mongod/mongos有效；同上，表示mongos或者mongod与其他mongod实例之间的连接池的容量，根据host限定。

# 配置样例

```
systemLog:
    quiet: false
    path: /data/mongodb/logs/mongod.log
    logAppend: false
    destination: file
processManagement:
    fork: true
    pidFilePath: /data/mongodb/mongod.pid
net:
    bindIp: 127.0.0.1
    port: 27017
    maxIncomingConnections: 65536
    wireObjectCheck: true
    ipv6: false	
storage:
    dbPath: /data/mongodb/db
    indexBuildRetry: true
    journal:
        enabled: true
    directoryPerDB: false
    engine: mmapv1
    syncPeriodSecs: 60 
    mmapv1:
        quota:
            enforced: false
            maxFilesPerDB: 8
        smallFiles: true	
        journal:
            commitIntervalMs: 100
    wiredTiger:
        engineConfig:
            cacheSizeGB: 8
            journalCompressor: snappy
            directoryForIndexes: false	
        collectionConfig:
            blockCompressor: snappy
        indexConfig:
            prefixCompression: true
operationProfiling:
    slowOpThresholdMs: 100
    mode: off
```

## 如果你的架构模式为replication Set，那么还需要在所有的“复制集”members上增加如下配置：

```
replication:
    oplogSizeMB: 10240
    replSetName: rs0
    secondaryIndexPrefetch: all
```

## 如果为sharding Cluster架构，则需要在shard节点增加如下配置：

```
sharding:
    clusterRole: shardsvr
    archiveMovedChunks: true
```

```
systemLog:
    quiet: false
    path: /data/mongodb/logs/mongod.log
    logAppend: false
    destination: file
processManagement:
    fork: true
    pidFilePath: /data/mongodb/mongod.pid
net:
    bindIp: 127.0.0.1
    port: 37017
    maxIncomingConnections: 65536
    wireObjectCheck: true
    ipv6: false	
replication:
    localPingThresholdMs: 15			
sharding:
    autoSplit: true
    configDB: m1.com:27018,m2.com:27018,m3.com:27018
    chunkSize: 64
```

> mongos实例不需要存储实际的数据，对内存有一定的消耗，在sharding架构模式下使用；mongos需接收向客户端请求（后端的sharded和replication set则不需要让客户端知道），它可以将客户端请求转发到一个分片集群上（分片集群基于复制集）延迟相对较小的secondary上，同时还负责chunk的分裂和迁移工作。

# repair修复

> “修复”数据库，当mongodb运行一段时间之后，特别是经过大量删除、update操作之后，我们可以使用repair指令对数据存储进行“repair”，它将整理、压缩底层数据存储文件，重用磁盘空间，相当于数据重新整理了一遍，对数据优化有一定的作用。

```
$ ./mongod --dbpath=/data/mongodb/db --repair
```

# mongodump与mongorestore

> 我们通常会使用到mongodb数据的备份功能，或者将一个备份导入到一个新的mongod实例中（数据冷处理），那么就需要借助这两个指令。

- 备份

```
>./mongodump --host m1.com --port 27017 -u root -p pass --out /data/mongodb/backup/dump_2015_10_10
```

- 还原

```
./mongorestore --db mydatabase /data/mongodb/backup/dump_2015_10_10  
```

# mongoimport和mongoexport

> mongoexport将数据导出为JSON或者CSV格式，以便其他应用程序解析。

# mongo shell

> 1）help：列出所有的function
> 2）show dbs：展示当前实例中所有的databases。
> 3）use <dbname>：切换到指定的db，接下来的操作将会在此db中。
> 4）show collections：展示出当前db中所有的collections。
> 5）show users：展示当前db中已经添加的所有用户。
> 6）show roles：展示当前db中所有内置的或者自定义的用户角色。
> 7）show profile：这涉及到profile相关的配置，默认情况下展示出最近5个操作耗时超过1秒的操作，通常用于跟踪慢查询。
> 8）db.help()：展示出可以在db上进行的操作function。
> 9）db.<collection>.help()：展示出可以在colleciton上进行的操作。


[我的博客](https://hans007.github.io)