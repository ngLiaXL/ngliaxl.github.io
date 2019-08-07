---
layout: post
title:  "[Android]	adb设置远程调试"
date:   2015-07-28 11:53:27 +0800
categories: android
---


1、`adb shell`

2、`su` 

3、`setprop service.adb.tcp.port 5555`

4、`adb tcpip 5555`回车，表示重启tcpip，并监听5555端口

5、`adb connect xx.xx.xx.xx`   `adb disconnect xx.xx.xx.xx`

