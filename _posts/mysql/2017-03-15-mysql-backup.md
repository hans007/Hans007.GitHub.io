---
layout: post
title: mysql - 数据备份
category: mysql
tags:
  - backup
---

# 编写备份sh

/opt/backup.sh

```sh
#!/bin/sh
cd /bak/bakmysql
echo "You are in bakmysql directory"
mv bakmysql* /bak/bakmysqlold
echo "Old databases are moved to wditcast folder"
Now=$(date +"%d-%m-%Y")
File=bakmysql-wditcast-$Now.sql
mysqldump -uroot -p'abc123' --databases wditcast > $File
echo "Your database backup successfully completed"
SevenDays=$(date -d -7day  +"%d-%m-%Y")
if [ -f /bak/bakmysqlold/bakmysql-wditcast-$SevenDays.sql ]
then
rm -rf /bak/bakmysqlold/bakmysql-wditcast-$SevenDays.sql
echo "You have delete 7days ago bak file "
else
echo "7days ago bak file not exist "
fi
```

> wditcast 改成你的数据库名字

# 设置可执行权限

```sh
chmod 755 /opt/backup.sh
```

# 设置任务

```sh
vi /etc/crontab
0 1 * * * root /opt/backup.sh

/sbin/service crond restart
```

> 这里设置为每天凌晨1点自动备份

