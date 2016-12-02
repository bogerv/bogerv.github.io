---
title: Yarn 基本使用
date: 2016-12-02 11:30:00
tags: [node, yarn, usage]
---

> Yarn 官网: https://yarnpkg.com

## Yarn 简介
Yarn 可以作为类似 npm 的包管理工具, 相比 npm 速度更快

<!--more-->

### **初始化**
```
yarn init
```
输入一些基本参数后, 会在目录下生成 project.json 文件

### **添加依赖项**
```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```
也可以使用开源项目的地址, 如 jquery-confirm
```
yarn add https://github.com/craftpip/jquery-confirm.git
```

### **升级依赖项**
```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

### **移除依赖项**
```
yarn remove [package]
```

### **安装所有依赖项**
```
yarn
```
或者
```
yarn install
```

> 更多 CLI Introduction: https://yarnpkg.com/en/docs/cli/