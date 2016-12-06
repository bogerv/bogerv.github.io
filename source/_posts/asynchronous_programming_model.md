---
title: Asynchronous Programming Model (APM)
date: 2016-12-06 10:55:08
tags: [IAsyncResult, async, .net]
---

也称为 IAsyncResult 模式, 异步操作需要使用 Begin*OperationName* 和 End*OperationName* 方法. (例如, FileStream类提供了BeginRead 和 EndRead (Read方法的异步版本) 方法来异步读取字节).

> 从 .NET Framework 4.0 开始, 任务并行库为异步和并行编程提供了新的模式. 详见: [Task Parallel Libary (TPL)](https://msdn.microsoft.com/zh-cn/library/dd460717(v=vs.110).aspx) 和 [Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/zh-cn/library/hh873175(v=vs.110).aspx)
因此, 不再建议使用此种模式.

<!--more-->

在调用完 **Begin***OperationName* 后, 应用程序可以继续在调用线程上执行其他操作, 同时异步操作在另外一个线程上执行. 每次调用 **Begin***OperationName* 时, 应用程序还应调用 **End***OperationName* 来获取操作的结果.

## 开始一个异步操作
**Begin***OperationName* 方法开始异步操作 *OperationName*, 并返回实现 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 接口的对象. [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx)存储有关异步操作信息. 异步操作信息如下:

| Member        | Description   |
| ------------- |:-------------:|
| [AsyncState](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.asyncstate(v=vs.110).aspx)      | 一个特定于应用程序的可选对象, 其中包含有关异步操作的信息 |
| [AsyncWaitHandle](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.asyncwaithandle(v=vs.110).aspx)      | 一个 [WaitHandle](https://msdn.microsoft.com/zh-cn/library/system.threading.waithandle(v=vs.110).aspx), 可用来在异步操作完成之前阻止应用程序执行      |
| [CompletedSynchronously](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.completedsynchronously(v=vs.110).aspx) | 指示异步操作是否是在用于调用 **Begin***OperationName* 的线程上完成, 而不是在单独的 ThreadPool 线程上完成      |
| [IsCompleted](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.iscompleted(v=vs.110).aspx) | 指示异步操作是否完成      |

**Begin***OperationName* 方法使用该方法同步版本签名中通过值或引用传递的任何参数. 并且不包含任何输出参数. **Begin***OperationName* 方法签名还包含两个附加参数. 
* 第一个参数定义了一个 [AsyncCallback](https://msdn.microsoft.com/zh-cn/library/system.asynccallback(v=vs.110).aspx) 委托, 当异步操作完成时调用委托所引用的方法. 操作完成时, 如果不想调用任何方法, 可以指定为 null.
* 第二个参数是用户自定义对象, 此对象可用来向异步操作完成时调用的方法传递应用程序特定的状态信息.

如果 **Begin***OperationName* 方法还采用其他一些操作特定的参数 (例如, 一个用于存储从文件读取的字节的字节数组), 则 [AsyncCallback](https://msdn.microsoft.com/zh-cn/library/system.asynccallback(v=vs.110).aspx) 和应用程序状态对象将是 **Begin***OperationName* 方法签名中的最后两个参数

**Begin***OperationName* 立即返回对调用线程 (主线程) 的控制. 如果 **Begin***OperationName* 方法抛出异常, 则会在开始异步操作之前抛出异常, 并且不会调用回调方法.

## 结束异步操作
**End***OperationName* 方法可结束异步操作 *OperationName*. **End***OperationName* 方法的返回值与其同步对应方法的返回值类型相同, 并且是特定于异步操作的. 例如, EndRead 方法返回从 [FileStream](https://msdn.microsoft.com/zh-cn/library/system.io.filestream(v=vs.110).aspx) 读取的字节数, [EndGetHostByName](https://msdn.microsoft.com/zh-cn/library/system.net.dns.endgethostbyname(v=vs.110).aspx) 方法返回包含有关主机的信息的 [IPHostEntry](https://msdn.microsoft.com/zh-cn/library/system.net.iphostentry(v=vs.110).aspx) 对象. **End***OperationName* 方法采用该方法同步版本的签名中声明的所有输出参数或引用参数. 除了来自同步方法的参数外, **End***OperationName* 方法还包括 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 参数. 调用方必须将对应调用返回的实例传递给 BeginOperationName. 
如果调用 **End***OperationName* 时 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 对象表示的异步操作尚未完成, 则 **End***OperationName* 将在异步操作完成之前阻止调用线程. 异步操作引发的异常是从 **End***OperationName* 方法引发的. 未定义多次使用同一 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 调用 **End***OperationName* 方法的效果. 同样, 也未定义使用不是相关 Begin 方法返回的 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 调用 **End***OperationName* 方法的效果. 

> 对于这两种未定义的情况, 实施者应考虑引发 [InvalidOperationException](https://msdn.microsoft.com/zh-cn/library/system.invalidoperationexception(v=vs.110).aspx). 

> 此设计模式的实施者应通知调用方异步操作已通过以下步骤完成: 将 [IsCompleted](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.iscompleted(v=vs.110).aspx) 设置为 true, 调用异步回调方法 ( 如果已指定一个回调方法 ), 然后发送 [AsyncWaitHandle](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.asyncwaithandle(v=vs.110).aspx) 信号. 

对于访问异步操作的结果, 应用程序开发人员有若干种设计选择. 正确的选择取决于应用程序是否有可以在操作完成时执行的指令. 如果应用程序在接收到异步操作结果之前不能进行任何其他工作, 则必须在获得这些结果之前先阻止该应用程序进行其他工作. 若要在异步操作完成之前阻止应用程序, 可以使用下列方法之一: 

* 从应用程序的主线程调用 **End***OperationName*, 阻止应用程序执行, 直到操作完成. 有关演示此方法的示例, 请参阅[Blocking Application Execution by Ending an Async Operation](https://msdn.microsoft.com/zh-cn/library/ms228967(v=vs.110).aspx). 
* 使用 [AsyncWaitHandle](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.asyncwaithandle(v=vs.110).aspx) 来阻止应用程序执行, 直到一个或多个操作完成. 有关演示此方法的示例, 请参阅[Blocking Application Execution Using an AsyncWaitHandle](https://msdn.microsoft.com/zh-cn/library/ms228962(v=vs.110).aspx). 

在异步操作完成时不需要阻止的应用程序可使用下列方法之一: 

* 按以下方式轮询操作完成状态: 定期检查 [IsCompleted](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult.iscompleted(v=vs.110).aspx) 属性, 并在操作完成后调用 **End***OperationName*. 有关演示此方法的示例, 请参阅[Polling for the Status of an Asynchronous Operation](https://msdn.microsoft.com/zh-cn/library/ms228968(v=vs.110).aspx). 
* 使用 [AsyncCallback](https://msdn.microsoft.com/zh-cn/library/system.asynccallback(v=vs.110).aspx) 委托来指定要在操作完成时调用的方法. 有关演示此方法的示例, 请参阅[Using an AsyncCallback Delegate to End an Asynchronous Operation](https://msdn.microsoft.com/zh-cn/library/ms228972(v=vs.110).aspx). 