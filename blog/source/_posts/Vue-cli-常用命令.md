---
title: Vue-cli 的使用
date: 2017-7-28 15:48:53
tags: Framework
---

## 全局安装脚手架
```
npm install -g vue-cli
```
<!-- more -->

如果速度慢，安装淘宝镜像,在全局安装脚手架：
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 项目初始化
```
vue init webpack Vue-Project
```

## 安装项目依赖
```
npm install
```

## 启动项目
```
npm run dev
```

启动时如果出错很可能就是port被占用了，进入/config/index.js修改port为其它端口号即可。

## 项目打包
```
npm run bulid
```
<br/>
![vue-cli.jpg](vue-cli.jpg)