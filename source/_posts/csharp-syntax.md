---
title: C# 语法糖
date: 2016-12-02 11:30:00
categories: [dotnet]
---

## 语法糖

| 符号        | 说明           |
| ------------- |:-------------:|
| **??**      | 非空取前者值, 空取后者值 | 
| **?.**      |       |

### 0.0 示例 **??**
```csharp
var str = GetValue() ?? "test";
```
相当于
```csharp
var str = GetValue();
if(str == null)
{
    str = "test";
}
```

### 0.1 示例 **?.**