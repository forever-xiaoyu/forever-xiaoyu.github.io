---
title: Better-scroll 的使用
tags: better-scroll
category: 技术框架
date: 2019-03-25 20:11:04
toc: true
---


> better-scroll 是一个解决场景滚动需求的滚动插件，在 iscroll 的基础上扩展了额外的一些功能。
> 该框架基于原生js实现,不依赖其他框架,是一款功能比较丰富的轻量级框架。

<!-- more -->

## 安装
```
npm install better-scroll --save
```
在相应代码中引入
```
import BScroll from 'better-scroll'
```

## 常用配置
```
let scroll = new BScroll('.wrapper', {
  scrollX: true, // 开启横向滚动, 默认为 false
  scrollY: true, // 开启纵向滚动
  eventPassthrough: vertical, // 保留纵向原生滚动
  click: true, // 派发点击事件
  bounce: { // 边缘回弹动画
  	top: true,
  	bottom: true,
  	left: true,
  	right: true
  },
  observeDOM: true, // 会检测 scroller 内部 DOM 变化, 自动调用 refresh 方法重新计算来保证滚动的正确性, 它会额外增加一些性能开销
  scrollbar: { // 开启滚动条
  	fade: true // 滚动条是否渐隐
  },
  pullDownRefresh: { // 配置下拉刷新
    threshold: 50, // 下拉距离
    stop: 20 // 回弹停留距离
  },
  pullUpLoad: { // 上拉加载
    threshold: 50
  }
})
```

## 常用方法
### refresh()
作用：重新计算 better-scroll，当 DOM 结构发生变化的时候务必要调用确保滚动的效果正常。

### scrollTo(x, y, time, easing)
参数：
- {Number} x 横轴坐标（单位 px）
- {Number} y 纵轴坐标（单位 px）
- {Number} time 滚动动画执行的时长（单位 ms）
- {Object} easing 缓动函数，一般不建议修改，如果想修改，参考源码中的 ease.js 里的写法
作用：滚动到指定的位置 。

### scrollBy(x, y, time, easing)
参数：
- {Number} x 横轴距离（单位 px）
- {Number} y 纵轴距离（单位 px）
- {Number} time 滚动动画执行的时长（单位 ms）
- {Object} easing 缓动函数，一般不建议修改，如果想修改，参考源码中的 ease.js 里的写法
作用：相对于当前位置偏移滚动 x,y 的距离。

### scrollToElement(el, time, offsetX, offsetY, easing)
参数：
{DOM | String} el 滚动到的目标元素, 如果是字符串，则内部会尝试调用 querySelector 转换成 DOM 对象。
- {Number} time 滚动动画执行的时长（单位 ms）
- {Number | Boolean} offsetX 相对于目标元素的横轴偏移量，如果设置为 true，则滚到目标元素的中心位置
- {Number | Boolean} offsetY 相对于目标元素的纵轴偏移量，如果设置为 true，则滚到目标元素的中心位置
- {Object} easing 缓动函数，一般不建议修改，如果想修改，参考源码中的 ease.js 里的写法
作用：滚动到指定的目标元素。

### enable()
作用：立即停止当前运行的滚动动画。

### stop()
作用：启用 better-scroll, 默认 开启。

### disable()
作用：禁用 better-scroll，DOM 事件（如 touchstart、touchmove、touchend）的回调函数不再响应。

### destroy()
作用：销毁 better-scroll，解绑事件。

### on(type, fn, context)
参数：
- {String} type 事件名
- {Function} fn 回调函数
- {context} 函数执行的上下文环境，默认是 this
作用：监听当前实例上的自定义事件。如：scroll, scrollEnd, pullingUp, pullingDown等。
示例：
```
import BScroll from 'better-scroll'
let scroll = new BScroll('.wrapper')
function onScroll(pos) {
  console.log(`Now position is x: ${pos.x}, y: ${pos.y}`)
}
scroll.on('scroll', onScroll)
```

### finishPullDown()
作用：当下拉刷新数据加载完毕后，需要调用此方法告诉 better-scroll 数据已加载。

### openPullDown(config)
参数：
- {Object} config，参考 pullDownRefresh 的配置，默认为 true。
作用：动态开启下拉刷新功能。

### closePullDown()
作用：动态关闭下拉刷新功能。

### autoPullDownRefresh()
作用：自动触发下拉刷新。

### finishPullUp()
作用：当上拉加载数据加载完毕后，需要调用此方法告诉 better-scroll 数据已加载。

### openPullUp(config)
参数：
- {Object} config，参考 pullUpLoad 的配置，默认为 true。
作用：动态开启上拉加载功能。

### closePullUp()
作用：动态关闭上拉加载功能。

## 常用事件

### scrollEnd
参数：{Object} {x, y} 滚动结束的位置坐标
触发时机：滚动结束。

### touchEnd
参数：{Object} {x, y} 位置坐标
触发时机：鼠标/手指离开

### refresh
参数: 无

### pullingDown
触发时机：在一次下拉刷新的动作后，这个时机一般用来去后端请求数据。

### pullingUp
触发时机：在一次上拉加载的动作后，这个时机一般用来去后端请求数据。

## 常见问题

### 动态计算横向宽度
使用水平滚动时,需要为 content 容器设置宽度,通常需要动态计算水平宽度,并进行赋值操作,待 DOM 结构渲染后再执行 refresh 方法重新计算 better-scroll。

### content 最小高度
在一些场景下，会有 better-scroll content 容器高度小于 wrapper 容器高度的情况，这种情况下是无法触发 better-scroll 的事件的，可以通过计算 wrapper 高度来赋予 content 容器最小高度；也可以在特定场景下通过计算 content 的高度赋予 wrapper 小于 content 容器的高度的值。

### 无法滚动问题
在当前组件使用缓存的情况下，请求数据完成后，没有滚动 better-scroll，这时进行了路由跳转再返回当前组件的时候，会出现无法滚动的情况，可以通过在钩子函数中执行 refresh 来解决这类问题。

## 参考
[better-scroll 文档][1]

[1]: https://ustbhuangyi.github.io/better-scroll/doc/zh-hans/