---
layout: post
title: linux - centos6 开发环境包安装 devtoolset-2
category: linux
tags:
  - linux
  - centos6
---

# devtoolset-2

[Developer Toolset](http://linux.web.cern.ch/linux/devtoolset/)
[]

## 安装

```
yum install --nogpgcheck devtoolset-2

/opt/rh/devtoolset-2/root/usr/bin/gcc

scl enable devtoolset-2 'bash'
```

- 取消验证 你的key不对

```
yum install --nogpgcheck xxxxxxxx
```

- scl 没有需要安装 

```
yum install scl-utils
```
