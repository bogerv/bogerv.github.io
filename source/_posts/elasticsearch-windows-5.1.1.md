---
title: Elasticsearch 5.1.1 在 Windows 下安装配置
date: 2017-01-18 11:39:03
tags: [Elasticsearch,windows]
---
# 简介
关于 Elasticsearch 的介绍可以查看 [<<Elasticsearch 权威指南>>](https://www.gitbook.com/book/fuxiaopang/learnelasticsearch/details), 讲解的非常详细.

本文主要讲解, Elasticsearch 5.1.1 版本在 Windows 环境下的安装和中文分词插件配置

# 安装
由于 Elasticsearch 是基于 Lucene 的, 所以要依赖 Java 环境

* 安装最新版 JAVA, 由 [www.java.com](http://www.java.com/) 下载

    安装目录示例: `C:\Program Files\Java`
<!--more-->
* 配置 JAVA 环境变量

    新建系统变量 JAVA_HOME, 指向 JAVA 安装路径
    ![](/assets/elasticsearch/java_home.png)
    编辑系统变量中 Path ,新增 `%JAVA_HOME%\bin` 或 `%JAVA_HOME%\jre\bin`
    ![](/assets/elasticsearch/path_java_home.png)
* 下载 Elasticsearch 5.1.1 版本 zip 包, 可由 [elasticsearch.org/download](http://www.elasticsearch.org/download/) 下载, 解压相应目录
    ![](/assets/elasticsearch/elasticsearch511.png)

* 现在可直接在控制台运行 elasticsearch.bat, 就已经启动了 elasticsearch 服务, 可在浏览器中输入 localhost:9200 查看状态
    ![](/assets/elasticsearch/9200status.png)

# 安装插件
## 安装 elasticsearch-head
Git 地址 [elasticsearch-head](https://github.com/mobz/elasticsearch-head), 对于 Elasticsearch 5.x, elasticsearch-head 不再以插件的方式集成到 Elasticsearch 中, 而是作为独立的服务.

具体操作在项目的 ReadMe 文件中已经描述的很详细了, [查看设置](https://github.com/mobz/elasticsearch-head#running-with-built-in-server)

设置完成后, 打开对应地址可以看到如下结果:
![](/assets/elasticsearch/elasticsearch_head.png)
其中 index 和 db_news 是建立的索引

## 安装 elasticsearch-analysis-ik
Git 地址 [elasticsearch-head](https://github.com/medcl/elasticsearch-analysis-ik)

* 在 Elasticsearch 根目录中 plugins 下新建 ik 文件夹
* 根据对应关系, 选择下载 v5.1.1 的 Release zip 文件(当然也可以下载源码, 自己编译)
    ![](/assets/elasticsearch/ik_version.png)
* 将 zip 解压到新建立的 ik 文件夹下
    ![](/assets/elasticsearch/ik_detail.png)

安装完成后, 重启 Elasticsearch 服务就可以使用了.
