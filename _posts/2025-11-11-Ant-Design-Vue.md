---
title: Ant-Design-Vue 篇
date: 2025-11-11 21:00:03 +0800
tags: Vue
categories: 技术
---

# 常见问题
### a-checkbox 绑定 reactive 实例视图不更新
通过 class 创建了 reactive 实例，绑定了其中变量到 a-checkbox 上，当调用实例方法设置变量 checked 为 false 时不生效，这个问题排查了很久，因为一开始就没有往组件库上的问题想，最后代码我认为没有问题，就尝试给 checked 定义为 ref 变量，生效了，好坑
