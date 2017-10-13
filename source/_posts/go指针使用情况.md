---
title: Go 函数的接收类型何时应该使用指针
date: 2017-10-13 10:39:20
categories: [go]
---

对于刚入门 Go 的人来说, 为函数的 receiver 指定为 **值类型** 或 **指针类型** 是很困难的. 如果不知道如何取舍, 选择指针类型的 reveiver. 但是有时候 **值类型** 的 receiver 更加合适, 比如对象是一些轻量级的不变的 structs, 使用 **值类型** 的 receiver 会更加高效. 下面是列举了一些常用的判断指导.

## 使用指针类型

* 如果该函数会修改receiver
* 如果receiver是 `struct` 并且包含互斥类型 `sync.Mutex`，或者是类似的同步变量，receiver必须是指针，这样可以避免对象拷贝
* 如果receiver是较大的 `struct` 或者 `array`，使用指针则更加高效。多大才算大？假设 `struct` 内所有成员都要作为函数变量传进去，如果觉得这时数据太多，就是 `struct` 太大
* 如果receiver是 `struct`, `array` 或者 `slice`, 并且其中某个element指向了某个可变量，则这个时候receiver选指针会使代码的意图更加明显

## 不使用

* 如果receiver是 `map`、`func` 或者 `chan`
* 如果receiver是 `slice` 并且该函数并不会修改此 `slice`
* 如果receiver使较小的 `struct` 或者 `array`，并且其变量都是些不变量、常量，例如 `time.Time` , 因为 **值类型** 的 receiver 可以减少需要回收的垃圾量

> **最后，如果不确定用哪个，使用指针类型的receiver**