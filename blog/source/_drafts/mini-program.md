---
title: Mini Program
date: 2018-09-05 16:11:04
tags: Mini Program
category: fe
---

## Mini Program

	Mini Program

---------------

## app.json

app.json 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。

```
{
	//用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录。
	"pages": [
		/pages/demo/demo
	],
	//定义小程序所有页面的顶部背景颜色，文字颜色定义等
	"window": {
	    "backgroundTextStyle": "dark",
	    "navigationBarBackgroundColor": "#fff",
	    "navigationBarTitleText": "this is nav title",
	    "navigationBarTextStyle": "black",
	    "enablePullDownRefresh": true
	  },
	"tabBar": {
	    "color": "#433D33",
	    "selectedColor": "#CC2729",
	    "backgroundColor": "#fff",
	    "borderStyle": "white",
	    "list": [
	      {
	        "pagePath": "pages/index/index",
	        "text": "home",
	        "iconPath": "images/home.png",
	        "selectedIconPath": "images/home.png"
	      }
	    ],
	    "position": "bottom"
	  },
	  "networkTimeout": 5000
	}
}
```

## Launch

&#8195;微信客户端在打开小程序之前，会把整个小程序的代码包下载到本地。紧接着通过 app.json 的 pages 字段就可以知道你当前小程序的所有页面路径。
&#8195;这个配置说明在 QuickStart 项目定义了两个页面，分别位于 pages/index/index 和 pages/logs/logs。而写在 pages 字段的第一个页面就是这个小程序的首页（打开小程序看到的第一个页面）。
&#8195;于是微信客户端就把首页的代码装载进来，通过小程序底层的一些机制，就可以渲染出这个首页。
&#8195;小程序启动之后，在 app.js 定义的 App 实例的 onLaunch 回调会被执行。整个小程序只有一个 App 实例，是全部页面共享的。

## Statement Period

 * onLoad(Object query)
 页面加载时触发。一个页面只会调用一次，可以在 onLoad 的参数中获取打开当前页面路径中的参数。

 * onReady
 页面初次渲染完成时触发。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。

 * onShow / onHide
 页面显示/切入前台时触发
 页面隐藏/切入后台时触发

## Event Handling
小程序本身对于使用频率较高的页面事件进行了处理，可直接调用。

 * onPullDownRefresh
 &#8194;需要在app.json的window选项中或页面配置中开启enablePullDownRefresh。
 可以通过wx.startPullDownRefresh触发下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。
 &#8194;当处理完数据刷新后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。

 * onReachBottom
 &#8194;可以在app.json的window选项中或页面配置中设置触发距离onReachBottomDistance。
 &#8194;在触发距离内滑动期间，本事件只会被触发一次。

 * onPageScroll(Object)
 Object: scrollTop 页面在垂直方向已滚动的距离（单位px）

![universe](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3546744997,114540464&fm=11&gp=0.jpg)