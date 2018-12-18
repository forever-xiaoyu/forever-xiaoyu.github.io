---
title: 基于 canvas 的简易画板
date: 2017-9-5 14:08:05
tags: canvas
categories: JavaScript
---
 
	在线画板算是一个比较有意思的功能，就想到用canvas实现一个简易的画板。
<!-- more -->
开发过程中需要注意的点：

1. 在绘制线条时避免使用点的形式绘制（如fillRect），使用路径形式进行绘制，否则在鼠标移动过快线条可能会产生间断。
2. 为了满足线条两端的圆角形式，需要使用 lineCap：round属性值。
3. 每次绘制之前获取上一次的坐标，绘制到鼠标移动到的坐标。

附上实例效果：

{% codepen forever-xiaoyu gZwWNz light [result 420] %}
