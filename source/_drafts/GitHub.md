---
title: GitHub
date: 2023-6-26 20:30:21
tags: Git
categories: 常用工具
toc: true
---

> GitHub 一些常见问题和使用上的一些总结。

<!-- more -->

## GitHub Token

### token 过期重新生成
mac 端使用时若权限不够需要加 sudo
```bash
git config --system --unset credential.helper 
```
另外在钥匙串中删除 GitHub 相关信息，再次操作时会要求重新输入账号密码，直接使用重新生成的token即可。