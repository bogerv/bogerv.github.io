---
title: app.Use vs app.Run in ASP.NET Core middleware
date: 2016-12-02 11:30:00
categories: [dotnet]
---

> 原文链接: [app.Use vs app.Run in ASP.NET Core middleware](http://www.talkingdotnet.com/app-use-vs-app-run-asp-net-core-middleware/)

**app.Run** 和 **app.Use** 都应用于中间件, 到底有什么不同过呢?

## 00.00 不同之处
在 ASP.NET Core 中, 中间件是按照添加顺序执行的. 使用 **app.Use** 定义的中间件可能会调用下一个中间件组件.
而使用 **app.Run** 定义的中间件, 永远不会调用后续的中间件

## 00.01 从代码看
创建一个新的 ASP.NET Core 1.0 Mvc 应用, 并将 Startup 类中 Configure 方法的内容改为如下.
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    app.Use(async (context, next) =>
    {
        await context.Response.WriteAsync("<html><body>");
        await context.Response.WriteAsync("<div>Inside middleware defined using app.Use</div>");
        await next();
        await context.Response.WriteAsync("</body></html>");
    });

    app.Run(async context => { 
       await context.Response.WriteAsync("<div>Inside middleware defined using app.Run</div>"); 
    });

    app.Use(async (context, next) =>
    {
        await context.Response.WriteAsync("<html><body>");
        await context.Response.WriteAsync("<div>Another Middleware defined using app.Use</div>");
        await next();
        await context.Response.WriteAsync("</body></html>");
    });
    app.UseIISPlatformHandler(options => options.AuthenticationDescriptions.Clear());
    app.UseStaticFiles();
    app.UseIdentity();
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```
以上示例使用 **app.Use** 定义了 2 个内联中间件，使用 **app.Run** 定义了 1 个内联中间件. 

使用 **app.Use** 定义的第一个中间件将异步写入响应对象的HTML, 调用下一个中间件，再次写入关闭HTML. 而使用 **app.Run** 定义的中间件写入消息，然后返回.

在 **app.Run** 中间件之后，我们使用 **app.Use** 定义了另一个内联中间件，并且还使用了一些内置的中间件

## 00.02 运行结果
运行应用程序时的输出结果:
```
Inside middleware defined using app.Use
Inside middleware defined using app.Run
```

如之前描述, 定义在 **app.Run** 之后的中间件不会被执行. 这就是为什么第二个使用 **app.Use** 定义的中间件没有响应的原因.