---
title: hexo使用心得
date: 2018-07-25 12:22:38
tags: hexo
categories: fe
---

** Hexo **
hexo 是一个十分简洁的框架，上手也相对容易。

*****

## 使用前提	
Node.js
Git

## 使用npm安装Hexo
```
npm install -g hexo-cli

```

## 创建项目
```
hexo init <folder>
cd <folder>
npm install

```

## 常用指令
```
hexo new [layout] <title>	—————————— [layout] => [post, draft, page]
hexo publish [layout] <title>
hexo generate
hexo server

```

## hexo 配置部署
在 _config.yml 中 deploy 下进行配置
```
deploy: 
	type: git
	repo: <url>
	branch: master

```

## 开始部署
安装部署依赖
```
npm install hexo-deployer-git --save

```
开始部署
```
hexo deploy

```

## 主题
移步 [hexo主题](https://hexo.io/themes/) 选择主题

## 多端使用

![universe](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1532503444822&di=de5d99915c0f2abd91bba4d0ac9a2465&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F15%2F12%2F85%2F89M58PICwUr_1024.jpg "universe")