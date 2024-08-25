---
title: JavaScript小结
date: 2017-10-18 21:24:46
categories: 基础技术
tags: JavaScript
toc: true
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
### Promise ###
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

task.then(function(response) {

}).catch(function(error) {

})
```
Promise 中的异常处理函数有两种写法，可以放在 then 中的第二参数位置，也可以放在 catch 中捕获，写在 catch 中的好处是还可以捕获到 then 中出现的异常。

### async/await ###
ES2017 标准引入了 async 函数，使得异步操作变得更加方便。
async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
```
当在 forEach 这种有自身作用域的函数中循环使用 await 时，要声明 async

```
arr.forEach(async (item) => {
  await(item)    
})
```

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

## 对象数组提取指定 key 值
```
[{a: 1, b: 2}, {a: 3, b: 3}].map(item => item.a)
// [1, 3]

[{a: 1, b: 2}, {a: 3, b: 2}].map(item => { return { a: item.a }})
// [{ a: 1}, { a: 3 }]
```

## find/findIndex
数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

```
[1, 4, -5, 10].find((n) => n < 0) // -5

[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

```
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

## 固定背景
移动端有遮罩层，背景通常禁止滚动，需要固定，通常有下面几个方法
1. overflow: hidden
2. position: fixed
3. 阻止 touch 事件的默认行为

上面几种方法不会保留滚动的位置，会回到页面顶部，可以在 2 的基础上，记录一下 top 值。
取消遮罩层之后，使用 scrollTo 回到原位置。

## getBoundingClientRect
Element.getBoundingClientRect()方法返回元素的大小及其相对于视口的位置。

实际开发中发现，在部分浏览器中如微信、Chrome浏览器等环境中，使用 getBoundingClientRect 获取容器顶部距离，会有一些问题。
容器实际上已经使用 fix 定位置顶，但实际上获取到的 getBoundingClientRect().y 的值为 0.25 或者其它值。
所以对于判断置顶问题要给出一个合理的误差范围。
> 实测中，在部分内核下，getBoundingClientRect 返回的对象中并没有 x，y 值，尽量用 top 来替代 y，left 替代 x。

## 安卓 usb 调试问题
安卓使用 usb 调试，出现这样一个报错

> Because an app is obscuring a permission request, Settings can't verify your response

如果有悬浮球或侧边栏，关闭它，或者关闭其它有保持应用在最上层的权限关闭掉

## Js 后置加载
在某些情况下，我们会需要指定 js 后置加载
```javascript
function loadScript(url,callback){
　　var script=document.createElement('script');
　　　　script.type='text/javascript';
　　　　script.async='async';
　　　　script.src=url;
　　　　document.body.appendChild(script);
　　　　if(script.readyState){   //IE
　　　　　　script.onreadystatechange=function(){
　　　　　　　　if(script.readyState=='complete'||script.readyState=='loaded'){
　　　　　　　　　　script.onreadystatechange=null;
　　　　　　　　　　callback();
　　　　　　　　}
　　　　　　}
　　　　}else{    //非IE
　　　　　　script.onload=function(){callback();}
　　　　}
}

Jquery：使用 $.holdReady(true);  $.getScript();   $.holdReady(false)   3个函数实现$.holdReady(true);    //hold住，等待a.js加载，后续代码不能执行
$.getScript('a.js',function(){
　　$.holdReady(false);     //释放，a.js加载完成，继续执行后续代码
});

```

## H5 定位
H5 定位只有在安全来源的情况才才被允许，比较常用的 https，localhost, 127.0.0.1 都被认为是安全来源
```
function getLocation()
{
    if (navigator.geolocation)
    {
        navigator.geolocation.getCurrentPosition(showPosition, showError);
    }
    else
    {
        console.log("该浏览器不支持获取地理位置。");
    }
}

function showPosition(position)
{
    console.log("纬度: " + position.coords.latitude + " 经度: " + position.coords.longitude);    
}

function showError(error) {
    console.log(error);
}
```

## 阻止/允许背景滚动 
```
let lockMaskScroll = (function (bodyClass) {
  let scrollTop
  return {
    afterOpen: function () {
      scrollTop = document.scrollingElement.scrollTop || document.body.scrollTop
      document.body.classList.add(bodyClass)
      document.body.style.top = -scrollTop + 'px'
    },
    beforeClose: function () {
      if (document.body.classList.contains(bodyClass)) {
        document.body.classList.remove(bodyClass)
        document.scrollingElement.scrollTop = scrollTop
      }
    }
  }
})('dialog-open')
```

## clearInterval 如何优雅清除定时器
很多时候清除定时器都会先调用 clearInterval，然后把 timer 赋值为 null 或 undefined，只是不够优雅罢了，其实 clearInterval 调用时会返回 undefined，可以直接这样写
```
timer = clearInterval(timer)
```

## iPhoneX 相关问题
### 适配问题
通常遇到 iPhoneX 会给底部加 34 像素的 bottom，但是在 safrai 和 weixin 环境下，会有一个底部的前进后退导航栏，而这个导航栏本身是适配过的，所以需要对 safari 和 weixin 进行单独的处理

```
function addIphoneXMethod () {
  // 微信浏览器 history.length 默认为 1
  if (!this.scrollEvent) {
    if (this.isIphoneX && navigator.userAgent.indexOf('Safari') > -1) { // Safari
      this.isIphoneX = false
      this.scrollEvent = window.addEventListener('scroll', () => {
        this.iPhoneXMethod()
      }, false)
    } else if (this.isIphoneX && navigator.userAgent.indexOf('MicroMessenger') > -1) { // Weixin
      if (this.viewHeight < 724 && window.history.length > 1) {
        this.isIphoneX = false
        this.scrollEvent = window.addEventListener('scroll', () => {
          this.iPhoneXMethod()
        }, false)
      } else {
        this.scrollEvent = window.addEventListener('resize', () => {
          this.iPhoneXMethod()
        }, false)
      }
    }
  }
}

function iPhoneXMethod () {
  if (window.innerHeight > 724) {
    this.isIphoneX = true
  } else {
    this.isIphoneX = false
  }
}
```

### 获取屏幕高度
在 Chrome、Safari 等浏览器中，由于本身地址栏会有滚动隐藏的问题，获取到的屏幕高度也是实时变化的，所以在这些情况下，不能只获取一次屏幕高度。

### 页面刷新问题
实际开中发现，iphoneX 等部分机型(iphone7等)在返回页面的时候请求接口走缓存，无法拿到最新的数据，所以要在请求的时候加上时间戳。

### 时间戳转换
在 IOS 系统中，无法通过 '2020-3-3' 格式进行转换，需要使用 '2020/3/3' 形式进行转换
``` javascript
const dt = new Date('2020/3/3')
```

### CDN 依赖
使用外部 cdn 加载 js 时，要做好 cdn 挂掉的备用方案
```
<link href="//unpkg.com/XXX.js" rel="preload" as="script">
<script src="//unpkg.com/XXX.js"></script>
<script>window.XXX || document.write('<script src="../XXX.js"></script>')</script>
```

## Vue 篇
### 数据更新无法触发视图更新
由于 Vue 的限制，不能检测以下变动的数组：
- 当你利用索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
- 当你修改数组的长度时，例如： vm.items.length = newLength

为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果， 同时也将触发状态更新：
```
// Vue.set/$set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice`
example1.items.splice(indexOfItem, 1, newValue)
```
为了解决第二类问题，你也同样可以使用 splice：
```
example1.items.splice(newLength)
```

### keep-alive
keep-alive 实现点击刷新，返回不刷新的场景。

第一次为 keep-alive 赋值为 true 的时候无效，第二次返回的时候 keep-alive 才有效，目前的解决办法是 keep-alive 默认设置为 true，不进行修改或者赋值，在导航守卫中判断路由，来刷新数据。

注：使用上述解决方案，只能解决默认在当前页面产生此现象，当默认在其他页面的时候还是会复现如上 bug ，由于 keep-alive 本身的设计缺陷，keep-alive 只会在路由跳转之前才会生效，对于跳转之后的重新赋值不会在本次跳转中生效，所以使用 keep-alive 的话此问题无解。唯一的解决办法是将 keep-alive 固定设置为 true 不做修改，到 Vue 的源码，找到 keep-alive 的缓存，在某一生命周期中，根据需要强制将缓存清空来达到刷新页面的目的。

### 图片的异常处理
```
<img :src="imgUrl" :onerror='this.src="' + errorImg + '";'>

<img :src="imgUrl? imgUrl : errorImg">
```

### IOS 微信浏览器填坑
IOS 的微信浏览器
- 默认通过扫码第一次进入页面是没有历史记录的，这就意味着没有底部的导航栏，可以通过 window.history.length === 1 是否是第一次进入页面。
- 一旦有页面跳转行为（使用 replaceState 替换历史记录除外），就会出现底部导航栏。
- 当用户进行滑动操作时：
  - 若用户向下滚动，则会隐藏导航栏
  - 若用户向上滚动，则会显示导航栏
- 若进入页面时存在导航栏，导航栏会占用屏幕高度，使用 scrollTop 时需要注意：
  - 若容器高度等于屏幕高度，在上面的前提下需要再给容器高度加上导航栏的高度
  - 如需要进行特殊处理，可以通过 scroll 事件对导航栏进行监听
- 若页面不存在导航栏，如需进行特殊处理，可以通过 resize 事件对导航栏进行监听。

### IOS input
由于 IOS 的安全机制，用户通过非交互触发的 focus 事件无法生效；而在 webview 中，可以通过对 webview 的配置来实现自动聚焦。
解决办法如下：
- 使用 SPA ，共用同一个 input 可以避免出现手动聚焦的情况。

### 共用路由跳转无法刷新问题
在使用同一个路由不同参数的情况下，页面是无法刷新的，解决此类问题通常使用如下几种方案

1. 导航守卫判断
2. 路由监听
3. router-view 增加标识区分

1、2 中通过对路由的操作进而进行具体更新的方法，或者使用 this.$router.go(0) 进行路由刷新

### $nextTick
$nextTick 并不能保证 DOM 结构完全渲染完毕之后才执行，在使用的时候根据实际情况进行校验，如：
```
this.$nextTick(() => {
  if (this.$refs.dom) {
    // doSomething
  }  
})
```

### computed 相关问题
父组件传给子组件数据之前，子组件数据为空，若使用 computed 的方法，若数据为对象属性，则不能监听变化，无法更新视图，可以重新 setter 方法，或者使用 watch 监听解决。

### 多页面监听 VueX state 中的变量
有时会出现多个组件 watch 同一个 VueX 中 state 的值，这时会同时触发多个组件中对应的 watch 函数，可以通过判断当前路由来解决，而对于使用组件缓存视图和非缓存视图而言，可以在 keep-alive 的生命周期中增加一个判断标识来解决，如果是新视图，关闭缓存视图中的 watch 函数相关事件。
