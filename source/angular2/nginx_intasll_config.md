---
title: Nginx 在 Windows 下基本安装和配置
date: 2017-03-20 12:35:03
categories: [angular2]
---

## 0.0 下载 Nginx
[https://nginx.org/en/download.html](https://nginx.org/en/download.html)

## 1.0 安装 Nginx
将下载的 zip 解压到某个文件夹即可

如: D:\Applications\nginx

> 可以将此文件夹添加到系统环境变量中

## 2.0 启动 Nginx
Nginx 默认启用 80 端口, 如果此端口被占用, 则会出错(错误日志在 D:\Applications\nginx\logs 目录下). 此时我们可以修改默认配置(将  D:\Applications\nginx\conf\nginx.conf 中 80 修改为 8090 或其他)

直接运行 D:\Applications\nginx 目录下的 nginx.exe

或者

命令行进入 D:\Applications\nginx 目录, 运行 nginx.exe

## 3.0 检测是否成功
在浏览器中运行 localhost:8090, 可以看到欢迎页面

## 4.0 停止 Nginx
运行命令行

`nginx -s stop`

或者直接结束 Nginx 进程

`D:\Applications\nginx\logs\nginx.pid`记录进程 Id

## 5.0 Nginx 使用自定义配置文件
启动 Nginx 是默认使用 conf/nginx.conf 文件. 实际上是运行了 `nginx -c conf/nginx.conf` 命令

如果想用自定义配置文件可直接运行

`nginx -c conf/xxxxxx.conf`
