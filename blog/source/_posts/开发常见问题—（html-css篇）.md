---
title: 开发常见问题（html/css篇）
date: 2017-10-16 20:40:28
categories: fe
tags: html/css
---

    在日常开发中，时常会碰到各种各样的坑，本文总结了开发过程中碰到的常见问题。

----------

## margin 重叠问题 ##
这是一个比较常见的经常在开发中碰到的问题，这里就不多说了，常见解决办法：

```
1. 创建新的 BFC 如 overflow: hidden;
2. 使用 padding
3. 尽量使用同一方向的 margin
```
## placeholder 自定义样式 ##
之前开发项目中碰到的需求，兼容写法如下：

```
/* IE 9 及以下版本不支持 */
input:-ms-input-placeholder {

}
input:-moz-placeholder {

}
input::-moz-placeholder {

}
input::-webkit-input-placeholder {

}
```
## 伪类和伪元素 ##
这两者很容易混淆，说的简单的一点，两者的区别在于：

```
CSS 伪类用于向某些选择器添加特殊的效果。
CSS 伪元素用于将特殊的效果添加到某些选择器。
```
## title 超出省略 ##
在移动端标题超出部分还是比较常见的，兼容性也 ok：

```
display: -webkit-box;
overflow: hidden;
text-overflow: ellipsis;
word-break: break-all;
-webkit-box-orient: vertical;
-webkit-line-clamp: 1;
```
## scroll 自定义样式 ##

```
/* Chrome */
::-webkit-scrollbar    //滚动条整体部分
::-webkit-scrollbar-button   //滚动条两端的按钮
::-webkit-scrollbar-track   // 外层轨道
::-webkit-scrollbar-track-piece    //内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-thumb //滚动条里面可以拖动的那个
::-webkit-scrollbar-corner   //边角
::-webkit-resizer   ///定义右下角拖动块的样式

/* IE */
  scrollbar-base-color: #C0C0C0;
  scrollbar-base-color: #C0C0C0;
  scrollbar-3dlight-color: #C0C0C0;
  scrollbar-highlight-color: #C0C0C0;
  scrollbar-track-color: #EBEBEB;
  scrollbar-arrow-color: black;
  scrollbar-shadow-color: #C0C0C0;
  scrollbar-dark-shadow-color: #C0C0C0;
```
Chrome 几乎可以完整修改 scrollbar 样式，但是 IE 比较有局限性，只能修改颜色，如果想要完美兼容，那么只能自己模拟实现一个 scrollbar 了。

## IE 绝对定位被 img 覆盖问题 ##
有这样一个场景，给浏览器插入一张 img ，其中有一些链接，通过绝对定位的办法，定位到 img 对应的位置，在chrome，firefox 中均没有问题，但是在 IE 中测试发现，链接被 img 挡住了。遇到这种问题，可以分两种方式解决。

```
1. 使用 css 背景图片替换 img
2. 给链接添加如下属性：
    background: url(about:blank);
```
## IOS 元素点击触发高亮 ##
-webkit-tap-highlight-color 是一个 不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。

当用户点击iOS的Safari浏览器中的链接或JavaScript的可点击的元素时，覆盖显示的高亮颜色。

该属性可以只设置透明度。如果未设置透明度，iOS Safari使用默认的透明度。当透明度设为0，则会禁用此属性；当透明度设为1，元素在点击时不可见
```
-webkit-tap-highlight-color：color
```

## 超出宽度横向滚动 ##
通常情况下，使用 white-space 和 overflow 即可解决

## inline-block 间隙 ##
 * 父元素设置 font-size: 0
 * 调整 margin
 * 如果是 span ，标签之间不换行
 * 垂直方向可以使用 vertical-align: bottom

<br/>   

参考文献
====
1. [详解 CSS 属性 - 伪类和伪元素的区别][2]
2. [CSS | MDN][3]


  [1]: https://jsfiddle.net/api/mdn/
  [2]: https://segmentfault.com/a/1190000000484493
  [3]: https://developer.mozilla.org/zh-CN/docs/Web/CSS