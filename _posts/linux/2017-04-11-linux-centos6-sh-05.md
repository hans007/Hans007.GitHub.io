---
layout: post
title: linux - centos6常用shell
category: linux
tags:
  - linux
  - centos6
---

# centos6常用shell

## 网卡配置

- 查看ip
  
  ifconfig

- 改自动获取

  vi /etc/sysconfig/network-scripts/ifcfg-eth0
  ONBOOT=yes

- 静态

  1）编辑配置文件,添加修改以下内容

    vi  /etc/sysconfig/network-scripts/ifcfg-eth0   

    BOOTPROTO=static   #启用静态IP地址
    ONBOOT=yes  #开启自动启用网络连接
    IPADDR=192.168.21.129  #设置IP地址
    NETMASK=255.255.255.0  #设置子网掩码
    GATEWAY=192.168.21.2   #设置网关
    DNS1=8.8.8.8 #设置主DNS
    DNS2=8.8.4.4 #设置备DNS
    IPV6INIT=no  #禁止IPV6
    :wq!  #保存退出

  2）修改完后执行以下命令

    service ip6tables stop   #停止IPV6服务
    chkconfig ip6tables off  #禁止IPV6开机启动
    service network restart  #重启网络连接
    ifconfig  #查看IP地址

## 系统启动加载

vi /etc/rc.d/rc.local

## 查看端口占用

netstat -ntlp

## 查询进程

ps -ef | grep xxxx

## iptables防火墙

vi /etc/sysconfig/iptables
service iptables restart
/etc/init.d/iptables status

## 压缩

tar xvf wordpress.tar       ||解压tar格式的文件||
tar -tvf myfile.tar       ||查看tar文件中包含的文件 ||
tar cf toole.tar tool       ||把tool目录打包为toole.tar文件||
tar cfz xwyme.tar.gz tool      ||把tool目录打包且压缩为xwyme.tar.gz文件，因为.tar文件几乎是没有压缩过的，MT的.tar.gz文件解压成.tar文件后差不多是10MB ||
tar jcvf /var/bak/www.tar.bz2 /var/www/       ||创建.tar.bz2文件，压缩率高||
tar xjf www.tar.bz2       ||解压tar.bz2格式||
gzip -d ge.tar.gz       ||解压.tar.gz文件为.tar文件||
unzip phpbb.zip       ||解压zip文件，windows下要压缩出一个.tar.gz格式的文件还是有点麻烦的||
bunzip2 file1.bz2       ||解压一个叫做 ‘file1.bz2′的文件||
bzip2 file1       ||压缩一个叫做 ‘file1′ 的文件||
gunzip file1.gz       ||解压一个叫做 ‘file1.gz’的文件||
gzip file1       ||压缩一个叫做 ‘file1′的文件||
gzip -9 file1       ||最大程度压缩||
rar a file1.rar test_file       ||创建一个叫做 ‘file1.rar’ 的包||
rar a file1.rar file1 file2 dir1       ||同时压缩 ‘file1′, ‘file2′ 以及目录 ‘dir1′||
rar x file1.rar       ||解压rar包||
unrar x file1.rar       ||解压rar包||
tar -cvf archive.tar file1       ||创建一个非压缩的 tarball||
tar -cvf archive.tar file1 file2 dir1       ||创建一个包含了 ‘file1′, ‘file2′ 以及 ‘dir1′的档案文件||
tar -tf archive.tar       ||显示一个包中的内容||
tar -xvf archive.tar       ||释放一个包||
tar -xvf archive.tar -C /tmp       ||将压缩包释放到 /tmp目录下||
tar -cvfj archive.tar.bz2 dir1       ||创建一个bzip2格式的压缩包||
tar -xvfj archive.tar.bz2       ||解压一个bzip2格式的压缩包||
tar -cvfz archive.tar.gz dir1       ||创建一个gzip格式的压缩包||
tar -xvfz archive.tar.gz       ||解压一个gzip格式的压缩包||
zip file1.zip file1       ||创建一个zip格式的压缩包||
zip -r file1.zip file1 file2 dir1       ||将几个文件和目录同时压缩成一个zip格式的压缩包||
unzip file1.zip       ||解压一个zip格式压缩包||

## 目录操作

- 切换工作路径 cd
- 显示目录内容 ls
- 复制文件或目录，复制目录时，加上-r选项表示递归复制 cp
- 重命名/移动文件或者目录 mv
- 删除文件或目录，删除目录时，加上-r选项表示递归,加上-f选项表示强制删除并且不提醒 rm
- 创建目录,递归创建加上-p选项  mkdir
- 创建空文件，或者更新时间戳 touch
- 列出目录树 tree




