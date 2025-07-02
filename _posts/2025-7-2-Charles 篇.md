---
title: Charles 篇
date: 2025-07-02 21:00:03 +0800
tags: Charles
categories: 技术
---

# PC端抓包
### https
在安装证书时，要选择将所有证书都放入下列存储，然后选择受信任的根证书颁发机构，这样才会生效。

通常设置 SSl Proxying Setting 来配置要抓取的白名单和黑名单。

# 移动端抓包
手机和PC处于同一网络环境，在手机wifi中添加代理，代理到PC的本机地址，PC的 Proxy Setting 要启用 transparent HTTP proxying。

## https
以前的时候包括 Charles 的设置上还是提示访问 http://chls.pro 或本机地址加端口（默认8888）来下载证书，但是已经不能用了，后来查了一番现在要访问 http://ssl.charles 来下载证书了，不过有说法是IOS描述文件要用这个地址，安卓的没有尝试。

# 问题汇总
## 和其他代理工具冲突
Proxy -> External Proxy Setting 设置中勾选 Use external proxy servers 然后选择 Web Proxy 和 Secure Web Proxy 选项，地址填写代理工具的地址和端口即可。

## 手机wifi添加代理后Charles没有抓取到
1. 检查PC防火墙是否关闭
2. 检查是否处于同一网络环境

## IOS微信小程序无法抓取
目前使用Charles无解，其他代理工具待尝试。
