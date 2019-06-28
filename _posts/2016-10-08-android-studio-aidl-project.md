---
layout: post
title:  "AndroidStudio创建AIDL工程"
date:   2016-10-08 11:53:27 +0800
categories: android
---

AndroidStudio远程接口调用

1、新建aidl server aidl client 两个工程

2、server 新建 xxx.aidl 文件 rebuild 后app/generated/source/aidl/debug 会生成xxx.java 接口文件 

3、client 新建xxx.aidl 包名要和server端包名一致，工程main目录新建aidl目录，aidl目录新建包，包名是server的aidl文件包名 

4、编写server端远程service 

5、client端调用service


[Server源码地址](https://github.com/ngLiaXL/AIDLServer)

[Client源码地址](https://github.com/ngLiaXL/AIDLClient)




