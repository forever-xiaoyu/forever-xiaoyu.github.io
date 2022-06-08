---
title: Mini Programs
category: 技术框架
tags: 小程序
date: 2018-09-05 16:11:04
toc: true
---


## Mini Programs

	小程序上线以来，依附微信的社交场景及自身特性，迅速普及起来，各大公司产品都在跟进小程序的开发。

---------------

## app.json

app.json 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。
<!-- more -->

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

## Pit
### 1. modify the object property values
Sometimes we need to modify a value of the object instead of modifying the entire object.
 ```
 handler () {
    this.setData({
    	['Object.key']: value
    })
 }
 ```
### 2. modify one value of array

 ```
 handler () {
    let key = "arr[" + index + "]"
      this.setData({
	  	[key]: value
    })
 }
 ```

### 3. stop event bubbling
```
 <view catchtap='handler'></view>
```

### 4. ios8 compatibility of wxss
ios8 is not very good for flex compatibility, flx needs to complete the suffix.
```
.style { 
    display: -webkit-box; 
    display: -webkit-flex;
    display: flex;
 }
```

### 5. canvas notice
* When the drawn text is too long, we need to handle the line break yourself.
```
textByteLength(text, num) {
    let strLength = 0;
    let rows = 1;
    let str = 0;
    let arr = [];
    for (let j = 0; j < text.length; j++) {
      if (text.charCodeAt(j) > 255) {
        strLength += 2;
        if (strLength > rows * num) {
          strLength++;
          arr.push(text.slice(str, j));
          str = j;
          rows++;
        }
      } else {
        strLength++;
        if (strLength > rows * num) {
          arr.push(text.slice(str, j));
          str = j;
          rows++;
        }
      }
    }
    arr.push(text.slice(str, text.length));
    return [strLength, arr, rows]   //  [text byte length，text displayed per line，rows]
},
```

* Since drawing is an async process, use canvasToTempFilePath api after drawing is complete.
```
ctx.draw(false, () => {
    //your code
})
```

* When using clip api to crop the image, there may be a problem that the clipping does not take effect, probably cause there is no stroke, which is a bug in the mini program.

### 6. wxParse related issues
wxParse can parse html code into mini program components, and the images rendered width wxParse can view large images.
But There're some problems:
* When images are many, the page is too laggy, affect user experience.
* Native rich-text reqires high inline style format, and easily render error.Meanwhile, it can't view large images.
* There is no perfect solution, using web-view may be better.

Sometimes we might meet situations where wxParse can't parse the content, the reason is in lines 112 and 119 of wxparse/html2json.js, there is a console.dir, comment out the function, the content can be resolved.

### 7. query node information
mini program is data-driven and don't support DOM operations, so you need to use a specific api to query related information.
```
handler () {
    let query = wx.createSelectorQuery();
    query.select("#ele").boundingClientRect();
    query.exec(function (res) {
    	//your code	
    })
}
```

### 8. swiper infinite sliding
if you use setData in bindchange event callback function to change the current value, it may cause setData to be called continuously.
Therefore, in general, please check the source field before changing the current value to determine whether it is caused by user touch.
```
swiperChange (e) {
    if(e.detail.source == "touch") {
        //your code
    }
}
```

### 9. create poster by wxCanvas
```html
<canvas canvas-id="shareCanvas" class='canvas-container' style="width:{{posterWidth}}px; height:{{posterHeight}}px;"></canvas>
```

```javascript
data: {
  posterWidth: 600,
  posterHeight: 900,
  hidePosterPopUp: true,
  isSavePoster: false
},

onLoad: function (options) {
  this.getSysInfo()
},

createPoster: function (e) {
  wx.showLoading({
    title: 'loading',
  })

  this.setData({
    hidePosterPopUp: !this.data.hidePosterPopUp,
  })

  let params = {
    // some config
  }

  this.makerPoster(params)
},

makerPoster ({
  content,
  img  
}) {
  const ctx = wx.createCanvasContext('myCanvas')

  // fill background
  ctx.setFillStyle('#ffffff')
  ctx.fillRect(0, 0, this.data.posterWidth, this.data.posterHeight)

  // fill text
  let clamp = ''
  var [contentLeng, contentArray, contentRows] = this.textByteLength(content, 40)
  contentArray.length > 3 && (clamp = '...')
  for (let i = 0; i < contentArray.length; i++) {
    if (i < 3) {
      this.drawText({
        ctx,
        fillText: contentArray[i] + (i === 2 ? clamp : ''),
        fontSize: 11,
        top: this.data.posterHeight * 0.05 + i * 15
      })
    }
  }

  // fill image
  ctx.save()
  ctx.beginPath()
  ctx.arc(
    this.data.posterWidth * 0.125,
    this.data.posterHeight * 0.87 + this.data.posterWidth * 0.075,
    this.data.posterWidth * 0.075,
    0, 2 * Math.PI
  )
  // clip img to a circle
  ctx.clip()
  this.drawImage({
    ctx,
    imgUrl: img,
    width: this.data.posterWidth * 0.15,
    height: this.data.posterWidth * 0.15,
    left: this.data.posterWidth * 0.05,
    top: this.data.posterHeight * 0.87,
  })
  ctx.restore()

  ctx.stroke()
  ctx.draw()
  wx.hideLoading()
},

drawImage ({
  ctx,
  imgUrl,
  width,
  height,
  left = 0,
  top = 0
}) {
  ctx.drawImage(imgUrl, left, top, width, height)
},

drawText ({ 
  ctx,
  fontSize = 14,
  textAlign = 'center',
  fillStyle = '#000000',
  fontWeight = 'normal',
  fillText,
  width = this.data.posterWidth / 2,
  top = this.data.posterHeight / 2
}) {
  ctx.font = `normal ${fontWeight} ${fontSize}px MicrosoftYaHei`
  // ctx.setFontSize(fontSize)
  ctx.setTextAlign(textAlign)
  ctx.setFillStyle(fillStyle)
  ctx.fillText(fillText, width, top)
},

textByteLength(text, num) {
  let strLength = 0; // text byte length
  let rows = 1;
  let str = 0;
  let arr = [];
  for (let j = 0; j < text.length; j++) {
    if (text.charCodeAt(j) > 255) {
      strLength += 2;
      if (strLength > rows * num) {
        strLength++;
        arr.push(text.slice(str, j));
        str = j;
        rows++;
      }
    } else {
      strLength++;
      if (strLength > rows * num) {
        arr.push(text.slice(str, j));
        str = j;
        rows++;
      }
    }
  }
  arr.push(text.slice(str, text.length));
  return [strLength, arr, rows]   //  [处理文字的总字节长度，每行显示内容的数组，行数]
},

getSetting: function() {
  var This = this;
  wx.getSetting({
    success(res) {
      if (!res.authSetting['scope.writePhotosAlbum']) {
        wx.authorize({
          scope: 'scope.writePhotosAlbum',
          success() {
            This.savePoster();
          },
          fail() {
            wx.hideLoading()
            wx.showToast({
              title: 'save failed, please try again later',
              icon: 'none',
              duration: 1000
            })
          }
        })
      } else {
        This.savePoster();
      }
    }
  })
},

savePoster: function () {
  let This = this
  wx.canvasToTempFilePath({
    canvasId: 'shareCanvas',
    success: function (res) {
      wx.saveImageToPhotosAlbum({
        filePath: res.tempFilePath,
        success: function () {
          This.setData({
            isSavePoster: true
          })
        },
        fail: function (err) {
          wx.showToast({
            title: 'save failed, please try again later',
            icon: 'none',
            duration: 1000
          })
        },
        complete: function () {
          wx.hideLoading()
        }
      })
    },
    fail: function (err) {
      wx.hideLoading()
      wx.showToast({
        title: 'save failed, please try again later',
        icon: 'none',
        duration: 1000
      })
    }
  }, this)
},

closePosterPopUp: function () {
  wx.hideLoading()
  this.setData({
    hidePosterPopUp: !this.data.hidePosterPopUp,
    isSavePoster: false
  })
},

getSysInfo: function () {
  wx.getSystemInfo({
    success: (res) => {
      console.log(res)
      let posterWidth = Math.round(res.screenWidth * 0.8)
      let posterHeight = Math.round(posterWidth * 1080 / 900)

      this.setData({
        posterWidth,
        posterHeight
      })
    },
  })
},
```

![universe](https://ss1.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3546744997,114540464&fm=11&gp=0.jpg)