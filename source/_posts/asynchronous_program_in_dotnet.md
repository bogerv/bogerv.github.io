---
title: 异步编程模式
date: 2016-12-05 11:30:00
tags: [async, .net, await]
---

## 三种异步编程模式
.NET Framework 提供了三种异步编程模式:
* APM (Asynchronous Programming Model): 异步编程模型
* EAP (Event-based Asynchronous Pattern): 基于事件的异步模式
* TAP (Tasks-based Asynchronous Pattern): 基于任务的异步模式

<!--more-->

### 异步编程模式 ( APM )

也称为 IAsyncResult 模式, 异步操作需要使用 Begin*OperationName* 和 End*OperationName* 方法. (例如, FileStream类提供了BeginRead 和 EndRead (Read方法的异步版本) 方法来异步读取字节).

> 从 .NET Framework 4.0 开始, 任务并行库为异步和并行编程提供了新的模式, 因此不再建议使用此种模式. 详见: [Task Parallel Libary (TPL)](https://msdn.microsoft.com/zh-cn/library/dd460717(v=vs.110).aspx) 和 [Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/zh-cn/library/hh873175(v=vs.110).aspx) 以及 [Asynchronous Programming Model (APM)](https://msdn.microsoft.com/zh-cn/library/ms228963(v=vs.110).aspx)

### 基于事件异步编程模式 ( EAP )

这种模式需要 Async 后缀, 也需要一个或多个事件、事件处理程序委托类型和 **EventArg** 派生类型. EAP 是在 .NET Framework 2.0 中引入的. 对于新的开发工作不再建议采用此模式. 有关详细信息, 请参阅[Event-based Asynchronous Pattern (EAP)](https://msdn.microsoft.com/zh-cn/library/ms228969(v=vs.110).aspx). 

### 基于任务异步编程模式 ( TAP )

使用一种方法来表示异步操作的启动和完成
TAP 是在 .NET Framework 4 中引入的, 并且它是在 .NET Framework 中进行异步编程的推荐使用方法. C# 中的 [async](https://msdn.microsoft.com/zh-cn/library/hh156513(v=vs.110).aspx) 和 [await](https://msdn.microsoft.com/zh-cn/library/hh156528(v=vs.110).aspx) 关键词为 TAP 添加了语言支持. 有关详细信息, 请参阅[Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/zh-cn/library/hh873175(v=vs.110).aspx). 

## 比较三种异步模式
为了快速比较这三种模式的异步操作方式，使用从指定偏移量处起将指定量数据读取到提供的缓冲区中的 **Read** 方法: 
```csharp
public class MyClass
{
    public int Read(byte [] buffer, int offset, int count);
}
```
* 此方法对应的 APM 将公开 BeginRead 和 EndRead 方法:
```csharp
public class MyClass
{
    public IAsyncResult BeginRead(
        byte [] buffer, int offset, int count, 
        AsyncCallback callback, object state);
    public int EndRead(IAsyncResult asyncResult);
}
```
* 对应的 EAP 将公开以下类型和成员的集
```csharp
public class MyClass
{
    public void ReadAsync(byte [] buffer, int offset, int count);
    public event ReadCompletedEventHandler ReadCompleted;
}
```
* 对应的 TAP 将公开以下单个 ReadAsync 方法
```csharp
public class MyClass
{
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);
}
```

## 相关连接
|标题|描述|
| ------------- |:-------------:|
|[Asynchronous Programming Model (APM)](https://msdn.microsoft.com/zh-cn/library/ms228963(v=vs.110).aspx)|描述使用 [IAsyncResult](https://msdn.microsoft.com/zh-cn/library/system.iasyncresult(v=vs.110).aspx) 接口提供异步行为的旧模型。 对于新的开发工作不再建议采用此模型|
|[Event-based Asynchronous Pattern (EAP)](https://msdn.microsoft.com/zh-cn/library/ms228969(v=vs.110).aspx)|描述提供异步行为的基于事件的旧模型。 对于新的开发工作不再建议采用此模型|
|[Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/zh-cn/library/hh873175(v=vs.110).aspx)|描述基于 [System.Threading.Tasks](https://msdn.microsoft.com/zh-cn/library/system.threading.tasks(v=vs.110).aspx) 命名空间的新异步模式。 此模型是在 .NET Framework 4 及更高版本中进行异步编程的推荐使用方法|


## 涉及的多线程概念
* 锁
* 死锁
* 征用条件