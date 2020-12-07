---
layout:	post
title:	"[React Native]	React Native JVerification"
date:	2020-12-07
---

[极光认证官方文档](http://docs.jiguang.cn/jverification/client/client_plugins/)

#  1、[极光官网创建应用](https://www.jiguang.cn/dev2/#/overview/appCardList)

- Android 输入应用包名和签名，签名使用极光提供的APK 获取
- iOS 输入Bundle ID
- RSA 加密公钥

#  2、RSA 生成秘钥对
- 生成私钥
```$ openssl genrsa -out rsa_private_key.pem 1024```
- 私钥转换成pkcs8格式
```$ openssl pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt -out pkcs8.pem```
- 生成公钥
```$ openssl rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem```

将`rsa_public_key.pem`内容复制到极光官网配置`RSA 加密公钥`，注意去掉头尾和中间换行符

# 3、运行demo

按照官网文档配置如下
### 1. 安装

```
npm install jverification-react-native --save
```

* 注意：如果项目里没有jcore-react-native，需要安装

  ```
  npm install jcore-react-native --save
  ```

### 2. 配置

#### 2.1 Android

* build.gradle

  ```
  android {
        defaultConfig {
            applicationId "yourApplicationId"           //在此替换你的应用包名
            ...
            manifestPlaceholders = [
                    JPUSH_APPKEY: "yourAppKey",         //在此替换你的APPKey
                    JPUSH_CHANNEL: "yourChannel"        //在此替换你的channel
            ]
        }
    }
  ```

  ```
  dependencies {
        ...
        implementation project(':jverification-react-native') // 添加 jverification 依赖
        implementation project(':jcore-react-native')         // 添加 jcore 依赖
    }
  ```

* setting.gradle

  ```
  include ':jverification-react-native'
  project(':jverification-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jverification-react-native/android')
  include ':jcore-react-native'
  project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')
  ```

#### 2.2 iOS

##### 2.2.1 pod

```
pod install
```

* 注意：如果项目里使用pod安装过，请先执行命令

  ```
  pod deintegrate
  ```
  
##### 2.2.2 配置AppKey

* App.js
```
const initParams = {
    'time': 5000,
    'appKey': 'yourAppKey',               //仅iOS
    'channel': 'channel',                 //仅iOS
    'advertisingId': 'advertisingId',     //仅iOS
    'isProduction': false,                //仅iOS
};
```
# 4、验证Token
- 客户端通过`JVerification.login`方法获取loginToken
- 客户端通过调用开发者服务器提供的接口去调用极光REST API `loginTokenVerify` 验证手机号码

# 5、遇到的问题
- V2.0.0 版本Android 无法替换登录按钮的样式，只能手动修改node_modules `jverification-react-native` 资源文件
- 登录按钮位置无法修改
- Android Applicaiton getPackages 增加  ```new cn.jiguang.plugins.verification.JVerificationPackage()```





	

