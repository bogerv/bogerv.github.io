---
title: ASP.NET MVC 源码控制里保持私有设置的 web.config 或 app.config
date: 2016-12-07 15:18:25
tags: [git, ASP.NET MVC]
---

> 原英文博客地址: [ASP.NET MVC: Keep Private Settings Out of Source Control](http://johnatten.com/2014/04/06/asp-net-mvc-keep-private-settings-out-of-source-control/#Use-configSource-Attribute-to-move-an-Entire-Configuration-Section-to-Its-Own-File)

在使用 git 管理 ASP.NET MVC 代码时, 当别人意外 push 了自己修改过的 web.config 或 app.config 文件, 我们每次在 pull 时总要修改数据库连接字符串或者一些其他私有设置. 很麻烦

现有以下解决方式:
* [使用 configSource Attribute 把整体配置部分移动到单独的文件中](#use-configSource-attr)
* [使用 File Attribute 有选择的移动 Application Settings (appSettings 标签下的设置) 到外部文件](#use-file-attr)
* [在 .gitignore 中添加特殊文件](#add-ignore)
* [Documentation is Key](#document-is-key)

<!--more-->

以 ASP.NET Web.config 文件为例:
```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
  A bunch of ASP.NET MVC web config stuff goes here . . . 
  -->
<configuration>
  <connectionStrings>
    <add name="DefaultConnection" value="YourConnectionStringAndPassword"/>
  </connectionStrings>
  <appSettings file="PrivateSettings.config">
    <add key="owin:AppStartup" value="AspNetIdentity2ExtendingApplicationUser.Startup,AspNetIdentity2ExtendingApplicationUser" />
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="EMAIL_PASSWORD" value="YourEmailPassword"/>
  </appSettings>
</configuration>
```

由于不同开发人员可能有不同的数据库连接字符串配置, 根据 .NET `ConfigurationManager` 提供了解决的方案. 

<span id="use-configSource-attr"></span>
## 使用 configSource Attribute 把整体配置部分移动到单独的文件中
我们可以添加单独的配置文件 ConnectionStrings.config, 并且在 Web.config 文件中的 `configSource` 特性中使用它.
* 新建 ConnectionStrings.config 文件
* 把先面的内容添加到 ConnectionStrings.config 中 (不需要 xml 头声明, 只有 `connectionStrings` 标签, 然后在标签里添加 `<add>` 元素)

    **ConnectionStrings.config 文件例子:**
    ```xml
    <connectionStrings>
    <add name="DefaultConnection" value="YourConnectionStringAndPassword"/>
    </connectionStrings>
    ```
* 修改 Web.config 文件, 移除 **<connectionStrings>** 下面的 **<add>** 元素, 在 **<connectionStrings>** 便签中添加 `configSource` 特性, 值为新创建的文件 (ConnectionStrings.config)

    **例子:**
    ```xml
    <connectionStrings configSource="ConnectionStrings.config">
    </connectionStrings>
    ```
**同以前访问连接字符串方式相同, 可直接使用如下代码访问:**
```csharp
var conn = ConfigurationManager.ConnectionStrings["DefaultConnection"];
string connString = conn.ConnectionString;
// Etc...
```

<span id="use-file-attr"></span>
## 使用 File Attribute 有选择的移动 Application Settings (appSettings 标签下的设置) 到外部文件
在 `<appSettings>` 标签里可以配置许多全局值供项目使用, 但有很多不需要提交到源码控制中 (如: 邮箱密码配置).
这种情况下, `<appSettings>` 标签里有一个特殊的 `file` 特性, 允许将一些配置放到独立的文件中.
* 添加新的文件 *PrivateSettings.config*, 也不带 XML 头, 只包含 `<appSettings>` 标签和其里面的内容
   
    **PrivateSettings.config文件:**
    ```xml
    <appSettings>
    <add key="MAIL_PASSWORD" value="123456"/>
    </appSettings>
    ```
* 移除 Web.config 中 `<appSettings>` 标签里要变为私有的内容, 并且添加 `file` 特性, 值为 *PrivateSettings.config*

  **修改后的 Web.config 文件:**
  ```xml
  <appSettings file="PrivateSettings.config">
    <add key="owin:AppStartup" value="AspNetIdentity2ExtendingApplicationUser.Startup,AspNetIdentity2ExtendingApplicationUser" />
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
  </appSettings>
  ```
**在项目中也可以直接使用如下代码来访问:**
  ```csharp
  var pwd = ConfigurationManager.AppSettings["MAIL_PASSWORD"];
  ```

<span id="add-ignore"></span>
## 在 .gitignore 中添加特殊文件
Now we can add our Web.config file to source and commit, and add the two special files, ConnectionStrings.config and PrivateSettings.config to our .gitignore file, and commit away. When it’s time to push to a shared repo, our private information will stay private.

<span id="document-is-key"></span>
## Documentation is Key
Of course, when we take this type of approach, it will be helpful to other developers if our documentation clearly indicates what is going on here. We might do this in our project README file, and/or add some XML comments at each point in our modified Web.config informing others that they will need to add the proper files to their local version of the project, and what those files should contain.