---
title: 关键字 readonly 和 const
date: 2016-11-29 17:37:22
tags: [C#, readonly]
---

使用 readonly 修饰一个属性时, 只能在此类中声明时或者构造函数体里为其赋值

> readonly 关键字和 const 不同. 一个 const 属性只能在声明的时候初始化. 而 readonly 还可以在构造函数中初始化.