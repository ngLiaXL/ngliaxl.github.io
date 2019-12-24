---
layout:	post
title:	"[Android]	AWS IoT"
date:	2019-12-23
---

### 1、创建用户池
用户池可以注册登陆服务，使用第三方社交登陆，配合身份池来授权用户能否使用AWS的服务。本文不使用任何类型的身份验证，可以忽略此步骤，如有需要可以访问如下链接使用身份池来验证身份。
[如何创建用户池](https://docs.aws.amazon.com/zh_cn/cognito/latest/developerguide/cognito-user-identity-pools.html)


### 2、创建身份池
- 打开AWS控制台->服务中找到Cignito->Create new identity pool->输入name->勾选 **Enable access to unauthenticated identities.**->Create pool.
![](/assets/aws-iot-02.jpg)
- 打开刚创建的身份池，记录身份池Id
![](/assets/aws-iot-06.jpg)


### 3、附加策略
- 身份池创建完成后，控制台会要求你在IAM上创建两个新角色，转到IAM ->角色并选择创建的角色
![](/assets/aws-iot-04.jpg)
- 选中已经创建好的角色**Cognito_android_testUnauth_Role**附加一条内联策略用来访问IoT服务
![](/assets/aws-iot-05.jpg)


### 4、创建物品
- 打开AWS控制台->服务中找到AWS IoT->管理->物品->创建
![](/assets/aws-iot-08.jpg)
交互里面有终端节点和一些定义好的Topic，记录终端节点。


### 5、创建策略
- 打开AWS控制台->服务中找到AWS IoT->安全->策略->创建
![](/assets/aws-iot-12.jpg)
可以直接录入* 记录策略名称


### 6、物品附加策略
- 打开AWS控制台->服务中找到AWS IoT->管理->物品->操作->附加策略
![](/assets/aws-iot-11.jpg)
至此AWS控制台配置完成。


### 7、使用Android SDK 订阅和发布消息

打开[AWS Android SDK Sample](https://github.com/awslabs/aws-sdk-android-samples/tree/master/AndroidPubSub)，
![](/assets/aws-iot-13.jpg)

- **awsconfiguration.json->PoolId和Region** 第2步身份池Id和区域
- **CUSTOMER_SPECIFIC_ENDPOINT** 第4步终端节点
- **AWS_IOT_POLICY_NAME** 第5步策略名称
打开APP录入自定义topic即可发送消息

### 8、AWS控制台订阅发布消息

- 打开AWS控制台->服务中找到AWS IoT->测试->录入订阅主题->录入指定要发布的主题和消息，QoS 为 0->发布到主题。


### 9、开发中遇到的问题
- 断网重连需设置`mqttManager.setAutoReconnect(true); mqttManager.setMaxAutoReconnectAttempts(Integer.MAX_VALUE);`
- 证书下载考虑到失败的场景需要做容错处理
- 初始化的clientId保证唯一，否则会有互相踢掉的情况。
- 采用匿名身份来访问AWS服务并不安全，建议使用身份验证来访问服务。
- 物品中可以创建证书，终端可以使用物品证书的方式使用IoT。
- 用户池和身份池用来限制用户能否使用AWS服务，IAM角色中内敛策略用来更细粒度的限制用户访问服务的权限


