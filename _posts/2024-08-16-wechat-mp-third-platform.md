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


DockerFile
```
# 二开推荐阅读[如何提高项目构建效率](https://developers.weixin.qq.com/miniprogram/dev/wxcloudrun/src/scene/build/speed.html)
# 选择构建用基础镜像。如需更换，请到[dockerhub官方仓库](https://hub.docker.com/_/java?tab=tags)自行选择后替换。
# 微管家 image 添加到开头 1/4
FROM ccr.ccs.tencentyun.com/weixincloud/weixincloud_wxcomponent:latest as wxcomponent

FROM maven:3.6.0-jdk-8-slim as build

# 指定构建过程中的工作目录
WORKDIR /app

# 将src目录下所有文件，拷贝到工作目录中src目录下（.gitignore/.dockerignore中文件除外）
COPY src /app/src

# 将pom.xml文件，拷贝到工作目录下
COPY settings.xml pom.xml /app/

# 执行代码编译命令
# 自定义settings.xml, 选用国内镜像源以提高下载速度
RUN mvn -s /app/settings.xml -f /app/pom.xml clean package

# 选择运行时基础镜像
FROM alpine:3.13

# 安装依赖包，如需其他依赖包，请到alpine依赖包管理(https://pkgs.alpinelinux.org/packages?name=php8*imagick*&branch=v3.13)查找。
# 选用国内镜像源以提高下载速度
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tencent.com/g' /etc/apk/repositories \
    && apk add --update --no-cache openjdk8-jre-base \
    && rm -f /var/cache/apk/*

# 容器默认时区为UTC，如需使用上海时间请启用以下时区设置命令
# RUN apk add tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

# 使用 HTTPS 协议访问容器云调用证书安装
RUN apk add ca-certificates

# 微管家文件 添加到最后一个FROM语句后面 2/4
COPY --from=wxcomponent /wxcloudrun-wxcomponent /wxcloudrun-wxcomponent
ENV GIN_MODE release

# 指定运行时的工作目录
WORKDIR /app

# 将构建产物jar包拷贝到运行时目录中
COPY --from=build /app/target/*.jar .

# 启动脚本复制到工作目录 3/4
COPY start.sh /app/

# 暴露端口
# 此处端口必须与「服务设置」-「流水线」以及「手动上传代码包」部署时填写的端口一致，否则会部署失败。
# 微服务管家端口为80 上传代码部署的时候 端口要填写80
EXPOSE 80

# 执行启动命令.
# 写多行独立的CMD命令是错误写法！只有最后一行CMD命令会被执行，之前的都会被忽略，导致业务报错。
# 请参考[Docker官方文档之CMD命令](https://docs.docker.com/engine/reference/builder/#cmd)
#CMD ["java", "-jar", "/app/springboot-wxcloudrun-1.0.jar"]
# 修改启动命令 增加 微管家服务启动项 4/4
CMD ["/bin/sh", "/app/start.sh"]
```





	

