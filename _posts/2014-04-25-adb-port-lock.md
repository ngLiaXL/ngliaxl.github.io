---
layout: post
title:  "[Android]	ADB"
date:   2014-04-25 11:53:27 +0800
categories: android
---


1、CMD命令窗口输入：`adb nodaemon server` 。查看哪个端口被占用。


2、输入 `netstat -ano | findstr "5037"` 。然后会弹出提示告诉你哪些进程占用了该端口，记住非0地址的后面的数字


3、打开任务管理器，点击“进程“，“查看”-“选择列”，勾选PID


4、查找第2步中看到的数字PID，然后结束相关进程，即可