---
title: 在 VS 中远程部署 ASP.NET MVC 项目到服务器
date: 2017-02-22 20:35:03
categories: [dotnet]
tags: [ASP.NET MVC,Visual Stduio]
---

## 前言
想必以前我们都有过将 ASP.NET MVC 项目部署到远程服务器的经历, 最普通也最麻烦的方法就是将发布后的项目打包上传到远程服务器, 操作起来非常不方便. VS 其实从很早就支持将项目 Publish 到远程的操作.
> 现在也有很多自动化部署工具, Jenkins, PowerShell 脚本等等, 暂不讨论

## 部署环境
Windows Server 2008 R2 Enterprise
Visual Studio 13 +

## 配置远程服务器

### 安装 Web Deploy
Web Deploy, 下载地址: [http://www.iis.net/downloads/microsoft/web-deploy](http://www.iis.net/downloads/microsoft/web-deploy)
> 安装 3.5 以上版本, 否则发布项目时会出项 "无法连接到远程服务器" 的错误提示

<!--more-->

### 启用 IIS 的管理服务
在管理服务中添加 IIS 的 Management Service 功能.

![](/assets/webdeploy/1.server_manager_add_features.png)

![](/assets/webdeploy/2.iis_management_service.png)

安装完成之后, 确保在 "服务" 中将其启动:

![](/assets/webdeploy/3.as_service_started.png)

> 如果该服务没有启动, 或者没有启用 "Management Service" 功能, 则在使用 Web Deploy 部署项目时会遇到如下错误: ![](/assets/webdeploy/4.error.png)

### IIS 设置
启用管理服务后, 进入 IIS, 会多出 Management Service 选项:

![](/assets/webdeploy/5.iis_home.png)

双击进行配置:

![](/assets/webdeploy/6.management_service_setting.png)

### 配置远部署项目的账户
双击 IIS Manager Users:

![](/assets/webdeploy/7.iis_user.png)

点击 Add User...:

![](/assets/webdeploy/8.iis_user_add.png)

输入用户名密码:
> 在 VS 中发布是也需要此次输入的用户名和密码

![](/assets/webdeploy/9.iis_user_name_pass.png)

### 配置站点用户权限
点击要发布的站点, 然后选择 IIS Manager Persmissions:

![](/assets/webdeploy/10.iis_user_permission.png)

之后点击 "Allow User" :

![](/assets/webdeploy/11.iis_allow_user.png)

将之前添加的 test 用户加入:

![](/assets/webdeploy/12.select_user.png)

### 为站点目录添加本地服务的修改权限
"LOCAL SERVICE" 账户要有修改 项目发布文件夹 的权限:

![](/assets/webdeploy/17.floder_peimission.png)

![](/assets/webdeploy/18.full_setting.png)


## 使用 Visual Studio 2015 远程部署项目
新建一个配置文件:

![](/assets/webdeploy/13.pubish.png)

配置 Web Deploy:

![](/assets/webdeploy/15.connection.png)

点击检查就可以了:

![](/assets/webdeploy/16.check.png)

> 默认会在本地浏览器打开
