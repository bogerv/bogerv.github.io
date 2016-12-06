---
title: Markdown 常用语法
date: 2016-10-08 14:37:22
tags: [markdown]
---
## 标题
标题可分为六级标题, # 逐级递加.
```
# 一级标题
## 二级标题
### 三级标题
#### 司机标题
##### 五级标题
###### 六级标题
```

<!--more-->

## 列表
#### 无序列表
无序列表以 `*` 或者 `-` 开头
```
* 文本1
* 文本1
* 文本1

- 文本1
- 文本1
- 文本1
```
#### 有序列表
有序列表以 `1.` `2.` `3.`开头
```
1. 文本1
2. 文本2
3. 文本3
```

## 链接和图片
#### 链接
```
[Google](www.google.com)
```
#### 锚点
跳转到<a href="#abcd">bookmark_text</a>位置
或者
跳转到[abcd](#abcd)位置
```
跳转到<a href="#abcd">bookmark_text</a>位置
或者
跳转到[abcd](#abcd)位置
```

<span id="abcd">我是abcd</span>

#### 图片
```
![](http://bogerv.wang/favicon.png)
```

## 引用
```
> 披繡闥, 俯雕甍, 山原曠其盈視, 川澤紆其駭矚. 閭閻撲地, 鐘鳴鼎食之家;
> 舸艦迷津, 青雀黃龍之舳. 雲銷雨霽, 彩徹區明. 落霞與孤鶩齊飛, 秋水共長天一色.
> 漁舟唱晚, 響窮彭蠡之濱, 雁陣驚寒, 聲斷衡陽之浦.
```

## 粗体和斜体
```
**粗体**
*斜体*
```

## 代码引用
#### 单行引用
```
`console.log('hello, world!')`
```
#### 多行引用
```
`` ` csharp
var hello = "hello, world!";
System.WriteLine(hello);
`` `
`` ` javascript
var hello = 'hello, world!';
console.log(hello);
`` `
注: 去掉上面`` `中的空格
```

## 表格
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```

## 显示链接中带括号的图片
```
![][1]
[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1
```