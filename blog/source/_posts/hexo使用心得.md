---
title: Hexo 使用心得
date: 2018-07-25 12:22:38
tags: Hexo
categories: Framework
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
<!-- more -->

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

## 开启图片上传
在配置文件中启用设置
```
post_asset_folder: true
```
然后安装相关插件
```
npm install hexo-asset-image --save
```
创建文章会生成同名文件夹，只需要将图片放在该路径中，使用时使用相对路径引用即可，eg：
```
<div align=center>![test.png](article/test.png)
```

## 开启搜索
需要安装 hexo-generator-searchdb，安装后到配置文件中开启相关选项即可
```
npm install hexo-generator-searchdb --save
```

## Markdown 语法
hexo 是使用 Markdown 编辑文章的，使用简洁，详见 [Markdown 官方文档][1]

## 注意事项
* 使用 git clone 修改 hexo 主题时，如果是在 hexo 项目中，删掉主题中的 .git 文件
* 使用 hexo new page 生成单页面
* 修改语言时，依据当前主题的 languages 中的语言进行修改，每个主题都会有所差异
<br/><br/>


![universe](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1532503444822&di=de5d99915c0f2abd91bba4d0ac9a2465&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F15%2F12%2F85%2F89M58PICwUr_1024.jpg "universe")

[1]: https://markdown-zh.readthedocs.io/en/latest/