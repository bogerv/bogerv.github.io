---
title: C# 中的协变性、逆变性和不变性
date: 2017-3-18 17:35:03
tags: [C# Syntax]
categories: [dotnet]
---

# C# 中的协变性、逆变性和不变性

## 协变和逆变的概念&ensp;

* 协变性: 子类转化为父类 string -> object

* 逆变性: 父类强制转换为子类 object -> string 或者 object -> (object)string

* 不变性: 既不可以协变也不可以逆变 (List<T> || 自定义泛型类型不加 out 或 in 修饰)

## 泛型类型的可变性
### 泛型修饰符

> out (返回值): 对于泛型参数, out 关键字指定给类型参数是协变的.

> in (输入参数): 对于泛型参数, in 关键字指定改类型那个参数是逆变的.

* 可以在泛型接口或泛型委托中使用
* 泛型参数中 in 和 out 可以共同存在

<!--more-->

### 框架定义的泛型委托
* `Action<T>` (T 支持逆变)
* `Func<TResult>` (T 支持协变)
* `Comparison<T>`
* `Predicate<T>`
```
public delegate TResult Func<out TResult>();
public delegate void Action<in T>(T obj);
```

### 自定义泛型委托


### 框架定义的泛型接口
* `IEnumerable<T>` (T 支持协变)
* `IEnumerator<T>` (T 支持协变)
* `IQueryable<T>` (T 支持协变)
* `IGrouping<TKey, TElement>` (TKey 和 TElement 支持协变)
* `IComparer<T>` (T 支持逆变)
* `IEqualityComparer<T>` (T 支持逆变)
* `IComparable<T>` (T 支持逆变)

### 自定义泛型接口

### 思考
#### <p style="color:red;"> 为什么 out(返回值) 只能协变? </p>
```
IEnumerable<object> obj = new List<object>();
//如果在这里我想obj中添加
IEnumeralbe<string> str = obj;
```
#### <p style="color:red;"> 为什么 out(返回值) 只能协变? </p>

