---
title: 使用Areas分离ASP.NET MVC项目
date: 2016-11-29 17:37:22
tags: [asp.net, mvc, area, route]
---

> 原文地址: http://www.cnblogs.com/dingji/archive/2012/05/30/2506420.html

## 使用Areas分离ASP.NET MVC项目
随着项目规模的不断扩大, Controller 控制器也随之不断增多. 如果在 Controllers 文件夹下面有超过两位数, 即便采用良好的命名规范, 或者用子文件夹的形式区分不同功能的控制器, 还是会影响项目的可阅读性和可维护性. 因此, 在一些场景下, 如果能把与某功能相关的文件分离到一个独立的项目中是非常有用的. Asp.Net MVC 提供了 Areas(区域) 的概念达到这一目的.

<!--more-->

### **1. 新建 Areas**
在 MVC 项目上右键新建 Areas, 输入名字, 如: Admin

![](/assets/161129_usearea_01.png)

在Areas/Admin/Controllers文件夹下面新建HomeController并添加一个Index的方法和对应的View文件. 这里可以发现Areas的另一个好处：你可以在不同Areas下面添加相同名称的Controller. 当然, 如果你直接这么运行会得到一个错误：

![](/assets/161129_error_02.png)

> 这种情况需要修改一下AdminAreaRegistration.cs和Global.asax, 分别为路由加上命名空间限制：

**/Areas/Admin/AdminAreaRegistration.cs**
```
context.MapRoute(
     "Admin_default",
     "Admin/{controller}/{action}/{id}",
     new { action = "Index", id = UrlParameter.Optional },
     new string[] { "[项目名称].Areas.Admin.Controllers" }
 );
```

**/Global.asax.cs 或者 RouteConfig.cs**
```
routes.MapRoute(
    name: "Default",
    url: "{controller}/{action}/{id}",
    defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional },
    namespaces: new string[] { "[项目名称].Web.Controllers" }
);
```

### **2. 改进**
我们需要对现有项目进行一下改造：

1.  在解决方案上面新建一个MyMvcAreasDemo.Admin的MVC3项目, 并且删除Global.asax和Web.config两个文件

2.  在根目录新建一个AdminAreaRegistration的类, 输入如下内容：

```
public class AdminAreaRegistration : AreaRegistration
{
    public override string AreaName
    {
        get
        {
            return "Admin";
        }
    }
 
    public override void RegisterArea(AreaRegistrationContext context)
    {
        context.MapRoute(
            "Admin_default",
            "Admin/{controller}/{action}/{id}",
            new { action = "Index", id = UrlParameter.Optional }               
        );
    }
}
```

3.  删除MyMvcAreasDemo项目/Areas/Admin文件夹下面的AdminAreaRegistration.cs文件以及Controllers文件夹（包括HomeController）

4.  在MyMvcAreasDemo.Admin项目的Controllers里面新建一个HomeController

5.  记得保留MyMvcAreasDemo/Areas/Admin下面的Views, 并且在MyMvcAreasDemo项目里面引用MyMvcAreasDemo.Admin项目, 如图：

![](/assets/161129_update_03.png)

现在运行程序并访问/Admin/Home/Index可以发现Admin项目生效了. 这样, 我们可以将所有的与后台管理相关的Controller都放到这个新的项目中来. 但是很快你会发现, 一个新的“问题”又出现了：

当你在MyMvcAreasDemo.Admin里面的HomeController添加新的Action（例如List）, 然后习惯性在上面右键-"Add View"后, 你会发现新增的List.cshtml文件会出现在MyMvcAreasDemo.Admin/Views/Home下面, 然后当你访问/Admin/Home/List的时候浏览器会得到错误提示："The view 'List' or its master was not found or no view engine supports the searched locations…". 原来它只会在主程序MyMvcAreasDemo中的对应目录去寻找View. 这么一来, MVC框架提供给我们的脚手架功能就大打折扣, 当然你可以手动在MyMvcAreasDemo/Areas/Admin/Views中对应添加View, 或者在MyMvcAreasDemo.Admin项目中自动生成了View之后再拷贝过去. 有没有更好的办法呢？

### **3. 自动复制新生成的 View 到 Areas 的 View 文件夹下**

为了使我们在MyMvcAreasDemo.Admin自动生成的View自动同步到MyMvcAreasDemo/Areas/Admin/Views文件夹中，可以使用“生成事件（Build Event）”里的“Post-Build Event”，打开MyMvcAreasDemo.Admin的属性，修改如下图所示：

![](/assets/161129_autocopybuidevent_04.png)

生成事件为：

```
mkdir "$(SolutionDir)$(SolutionName)\Areas\Admin\Views"
xcopy "$(ProjectDir)Views" "$(SolutionDir)$(SolutionName)\Areas\Admin\Views" /S /E /C /Y
```

意思其实很简单，相信大家都能看得懂，就是完全复制MyMvcAreasDemo.Admin的Views文件夹下所有文件至MyMvcAreasDemo/Areas/Admin/Views中。

现在再次访问/Admin/Home/List就可以得到正确结果了，并且你可以发现List.cshtml已经被复制到MyMvcAreasDemo/Areas/Admin/Views/Home目录里。

### **4. 另一种思路**

可以直接把 MyMvcAreasDemo.Admin 这个项目创建在 Area 文件夹里, 这里就可以省去 步骤3 的自动复制了