---
title: GitHub Pages 启用 https
date: 2018-11-14 11:30:02
tags: GitHub
categories: 基础技术
toc: true
---

## 启用 https

首先进入到项目中，进入项目的设置项，找到 Github Pages，启用 Enforce HTTPS 就可以了。
<!-- more -->

## 自定义域名问题

启用 https 之前，需要先添加 CNAME 文件（内容为自定义域名的顶级域名，这样 GitHub 会自动为你绑定 www 子域名）到存储库中，GitHub 会根据 CNAME 值设置 Custom domain，但是在设置自定义域名的时候，出现了如下的警告：

![github](/assets/img/images/github.png)


“the CNAME is already taken”，在网上并没有多少相关的问题，于是给 GitHub Support 发了邮件：

![github](/assets/img/images/github-support.png)


很快收到 GitHub Support 的回复：

![github](/assets/img/images/github-support2.png)


大意是，需要在 DNS 解析记录中个新增一个 TXT 类型的记录，记录值为邮件中给出的内容，然后让其对此进行验证，验证通过后，GitHub Support 会告知你已经释放域名，之后就重新解析域名就可以了。

验证 TXT 记录值是否生效的办法如下,如果得到的响应值中有 GitHub Support 邮件中的内容，则表示添加成功：

```
//Windows下进入DOS模式后用以下命令:
nslookup -type=txt 域名

//Unix操作系统下用：
dig -t txt 域名
```
