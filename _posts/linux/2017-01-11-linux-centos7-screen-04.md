---
layout: post
title: linux - SSH远程会话管理工具 - screen
category: linux
tags:
  - linux
  - centos7
---

# 安装

yum install screen

# 创建screen会话

screen -S xxx

# 暂时离开，保留screen会话中的任务或程序

可以用快捷键Ctrl+a d(即按住Ctrl，依次再按a,d)

# 恢复screen会话

screen -r xxx

# 当前screen列表

screen -ls

# 关闭screen的会话

exit

# 远程演示

screen -x xxx

# 常用快捷键

Ctrl+a c ：在当前screen会话中创建窗口
Ctrl+a w ：窗口列表
Ctrl+a n ：下一个窗口
Ctrl+a p ：上一个窗口
Ctrl+a 0-9 ：在第0个窗口和第9个窗口之间切换


[我的博客](https://hans007.github.io)