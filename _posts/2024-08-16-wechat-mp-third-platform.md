---
layout:	post
title:	"[WeChat] 搭建微信小程序第三方平台"
date:	2024-08-16
---


## 第三方平台配置

### 配置
- [申请第三方平台](https://open.weixin.qq.com/)，启用`云开发`模式
- 设置白名单IP地址列表（接口调用使用）;授权测试公众号/小程序列表（未全网发布时测试用，不设置不能授权）
- [绑定开发小程序](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/operation/thirdparty/dev.html)
- 使用官方提供的[wxcloudrun-springboot](https://github.com/WeixinCloud/wxcloudrun-springboot)项目部署到云环境中
- 服务商管家-开发辅助-消息转发器中设置消息转发器接口，并在项目[wxcloudrun-springboot]中新增接口接收 ticket 转发至业务服务器
- 服务商管家-管家中心-授权链接生成器-设置授权回调url
- 服务商管家-管家中心-授权链接生成器-获取授权链接


### 三方平台小程序绑定流程
-   `服务商微管家`中设置授权链接和回调
-   浏览器打开授权链接跳转至授权二维码页面，用户扫码后会回调到步骤1设置的回调地址
-   回调地址页面会获取微信授权的`authCode`，拿到 `authCode` + 业务id 调用后台接口实现绑定
-   返回到业务绑定页面，调用接口查询绑定状态，刷新页面
-   回调URL code 可不使用 回调url和授权事件通知功能一样 通知没有业务标识 选择回调


### 提交代码
-   开发小程序提交代码，到`草稿箱`后生成模版，通过接口的方式提交生成体验版发布
-   [设置第三方平台服务器域名](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/api/ThirdParty/domain/modify_server_domain.html)
-   [配置小程序服务器域名(先调用 ‘设置第三方平台服务器域名’ 否则报错)](https://developers.weixin.qq.com/doc/oplatform/openApi/OpenApiDoc/miniprogram-management/domain-management/modifyServerDomain.html)
-   [设置小程序用户隐私保护指引](https://developers.weixin.qq.com/doc/oplatform/openApi/OpenApiDoc/miniprogram-management/privacy-management/setPrivacySetting.html)
-   [发布已通过审核的小程序](https://developers.weixin.qq.com/doc/oplatform/openApi/OpenApiDoc/miniprogram-management/code-management/release.html)

### extAppid    
- 指的是授权给第三方平台的商家小程序的appid。
- 当服务商希望在小程序模板代码中结合某个商家小程序进行个性化的开发和调试，即可通过该参数来完成，详情可查看第三方平台代开发说明
```
ext_json {"extAppid":'', "ext": {}, "window": {}} 标准模板
{
    "extEnable": true,
    "extAppid": "wx5852b84f5f6f6c8e",
    "directCommit": false,
    "ext": {
        "host_url": ""
    }
}
```



## wxcloudrun-springboot
第三方平台对接业务系统 Java SpringBoot版本

### [项目地址](https://github.com/WeixinCloud/wxcloudrun-springboot)

### [将业务系统与服务商管家对接](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/product/wxcloudrun_dev.html)
-   根据文档修改`Dockerfile`文件
-   **服务商管家默认端口号80 EXPOSE端口和部署发布时端口号都填写80，否则打开主页不会进入微管家，业务系统端口号修改为其他的不能占用80端口**
-   **启动脚本复制到工作目录 `COPY start.sh /app/` 否则发布会找不到启动脚本，微信启动docker的时候会映射到填写的端口号 80**



### 服务商管家配置
-   `消息转发器`:微信官方推送的消息转发给内部业务服务
-   `proxy配置`:外部请求透传转发至内部业务服务

### 使用注意
运行时环境变量需配置到云托管`服务管理`>`服务列表`>`服务设置`中
- MYSQL_ADDRESS     
- MYSQL_PASSWORD    
- MYSQL_USERNAME   
- WX_APPID         
- MYSQL_DATABASE 第三方平台默认数据库 `wxcomponent`


### docker相关
查看container
`$ docker ps -a `

删除所有 container
`$ docker rm -f $(docker ps -aq) `

查看镜像
`$ docker images`
删除所有镜像
`$ docker rmi -f $(docker images -q)`

构建镜像
`$ docker build -t test:latest . ` 

`$ docker run -p 8000:8089 test:latest`
`$ docker run test:latest`
`$ docker run  -p 8090:80 test`  表示将容器内部的端口 80 映射到宿主机的端口 8090 之后机器访问 8090就会放到到docker container 80端口





	

