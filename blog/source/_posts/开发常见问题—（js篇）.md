---
title: 开发常见问题（js篇）
date: 2017-10-18 21:24:46
categories: Others
tags: JavaScript
---

	在日常开发中，时常会碰到各种各样的坑，本文总结了开发过程中碰到的常见问题。

----------

## 配置 Swiper ##
Swiper 是一个使用率很高的插件，这里说一些比较陌生的属性与方法：
<!-- more -->

```
//活动块居中
centeredSlides: true,
//触摸距离与 slide 滑动距离的比率
touchRatio: 0.5,
//当 swiper 样式或者子元素改变时，会自动初始化 swiper ，默认 false
observer: true,
//swiper从当前slide开始过渡到另一个slide时执行。触摸情况下，如果释放slide时没有达到过渡条件而回弹时不会触发这个函数
onSlideChangeStart: function(swiper) {
    //获取当前索引
    console.log(swiper.realIndex);
},
//回调函数，当你轻触(tap)Swiper后执行。
onTap: function(swiper) {
    //获取 tap 索引
    mySwiper.slideTo(swiper.clickedIndex);
}
```

## 移动端键盘遮挡问题 ##
有些时候，如：需要将某一 button 固定到屏幕底部，这个时候如果弹出键盘，那么 button 则会跟着跑到键盘上面（无论是 absolute 还是 fixed），所以可以在 focus 的时候，隐藏 button，而在 blur 的时候显示 button ，但是这时有一个问题：在安卓手机上没有问题，收起键盘可以出发 blur 事件，但是 IOS 端则没有触发，所以解决办法是，可以改用 resize 事件替代 blur 事件。


## 异步回调解决方案 ##
异步处理一直以来都是 js 极其重要的一部分，这里暂不提框架，原始的异步处理一般是：
    创建异步对象XMLHttpRequest。
    设置请求参数（请求方式，请求资源的相对路径，是否异步）。
    设置回调函数，用来处理服务器响应，使用onreadystatechange。
    获取异步对象的readyState属性，根据服务器返回状态信息判断是否请求成功。
如果存在多个请求，且数据相互有依赖关系的话，代码写起来会比较复杂，看起来杂乱无章，同时不利于后期维护。在 ES6 中，提出了 Promise 的概念，给出了异步回调的解决方案：

```
var task = new Promise(function(resolve, reject) {
 if (/* 异步操作成功 */){
     resolve(result);
 } else {
     reject(error);
 }
});

task.then(function(response) {
 // success
}, function(error) {
 // fail
});

```
使用 Promise 可以通过链式调用避免层层嵌套，同时便于代码阅读和理解。

## input readyonly 点击出现光标 ##
在开发中使用了一个时间插件，设置了 input 为 readonly，在低版本（10.2） IOS中，点击 input 还是会出现光标，尝试了很多方式，最后解决办法：

```
<input type="text" readonly unselectable="on" onfocus="this.blur()" value=""/>
```
## IE9 change 事件剪切删除无法触发 ##
jQuery 的 change 事件，在使用的时候，发现 IE 中剪切和删除无法触发，可以使用 input propertychange 事件来代替。

## input 上传同一文件问题 ##
这是一个常在开发中碰到的问题，试了网上很多方法，最简单的方法：

1. 上传文件时，修改 input 上传类型
2. 上传完成后再将上传文件类型恢复
3. 若有form表单，可以通过 reset() 方法重置表单解决

## let 的一些用法 ##
先来说说 let 比较常见的特性：

1. let 用来声明变量，所声明的变量只在 let 命令所在的代码块内有效。块级作用域的出现，实习上使得应用广泛的 IIFE 不再必要了。
2. let 声明变量不存在变量提升，如果在使用之后声明，值为 undefined。
3. let 不允许在相同作用域内重复声明同一个变量。

在使用的过程，其实还有一个很方便的用法，如需要获取某一对象的几个属性，无需多次声明变量：
```
let { attr1, attr2 } = obj
```
## 限制小数正则 ##
有一段时间没有写正则表达式了，忘得差不多了，所以就把这个算成一个坑吧，需求是这样的：
需要实现一个输入框限制输入内容为数字，整数位不超过 8 位，小数位不超过 6 位，核心实现方式如下

```
var reg = /^\d{0,8}(?:\.\d{1,8})?/g;
```
"?:" 表示如果不满足条件不保存括号里的内容
"?"  表示尽可能少的匹配满足条件的内容
## DOM 操作的方法 ##
这里简单说一下插入节点，最直接的想到的就是 appendChild 和 insertBefore，其实还有一个很实用的方法：


insertAdjacentHTML/insertAdjacentText
该方法接收两个参数，分别是插入位置和插入内容，插入位置参数：
1. beforeBegin: 插入到标签开始前
2. afterBegin:插入到标签开始标记之后
3. beforeEnd:插入到标签结束标记前
4. afterEnd:插入到标签结束标记后

使用实例：
```
obj.insertAdjacentHTML("afterEnd","<input type='text'/>");  
```
## 根据对象字段排序 ##
```
var objs = {
    f: {
        id: 2,
        name: '2'
    }, 
    a: {
        id: 3,
        name: '3'
    }, 
    c: {
        id: 1,
        name: '1'
    }
};

// 自定义排序规则，按对象的id排序
var sortedObjKeys = Object.keys(objs).sort(function(a, b) {
    return objs[b].id - objs[a].id;
});

// 按默认排序规则，按对象的key排序
var sortedObjKeys = Object.keys(objs).sort();

for (var index in sortedObjKeys) {
    console.log(sortedObjKeys[index]);
    console.log(objs[sortedObjKeys[index]]);
    console.log('----------');
}
```