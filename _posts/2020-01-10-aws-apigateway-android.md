---
layout:	post
title:	"[Android]	AWS API Gateway"
date:	2020-01-10
---


[官方文档地址](https://docs.aws.amazon.com/zh_cn/apigateway/latest/developerguide/how-to-generate-sdk-android.html)

### 1、创建API Gateway
登陆AWS 控制台->服务中找到API Gateway->右上角Create API

![](/assets/aws-apigateway-01.jpg)

输入API 名称->创建 API。

### 2、创建资源
选择刚刚创建的API->找到左侧操作按钮点击下拉->选择创建资源->输入资源名称，资源路径->创建资源
![](/assets/aws-apigateway-02.jpg)


### 3、创建方法
选择刚刚创建的资源->找到左侧操作按钮点击下拉->选择创建方法->根据你的API方法选择方法名
![](/assets/aws-apigateway-03.jpg)
-	集成类型勾选HTTP
-	终端节点URL录入API的请求URL
保存。


### 4、设置授权信息和URL查询字符串参数
-	选中方法->点击方法请求->授权处可以选择AWS-IAM授权方式。
-	添加查询字符串
![](/assets/aws-apigateway-04.jpg)


### 5、部署 API
操作按钮处->选择部署API.


