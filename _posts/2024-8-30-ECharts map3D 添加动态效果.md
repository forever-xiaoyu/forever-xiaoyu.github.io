---
title: ECharts map3D 添加动态效果
date: 2024-08-30 07:22:00 +0800
tags: ECharts
categories: 技术框架
---

## 调研过程
接到需求后，起初查阅了一番ECharts官方文档，发现官方文档中并没有相关方法可以添加动态效果，然后又调研了一番相关技术文章加上在明宇的帮助下，一共想出两种实现方案：
1. 通过动态修改 3dmap 的 label 的背景间接达到实现动画的效果
2. 放弃 ECharts，使用 Three.js 重写整个3d地图

最终在两个方案中选择了方案1，原因如下：
1. 用 Three.js 搭配 D3.js 去写 3D 类的效果固然更好，自由度非常高，但一切效果都要自己实现，大到地图轮廓，小到地图标签，加上完全不熟悉 Three.js。虽然找到了一些 3d 地图的 demo，但是即便如此，我认为修改起来成本也很高，几乎是走一步卡一步，我认为在有限的时间内无法高质量的还原原有 3d 地图。
2. 继续在原有技术上扩展，同样可以满足效果，相比 Three.js 效率更快，虽然在实现过程中会产生一些额外的问题，但这些问题也是可以通过手段来解决掉的。

整体思路已经确定下来，下面就是按照既定思路去实现动态效果了，在这个过程中，也是处在不断遇到问题不断解决问题中。

## 实现过程
3dmap 的 label 不单单可以设置为背景图片，同样可以设置为 dataURI、HTMLImageElement 对象或者 HTMLCanvasElement 对象，对于现有需求，我分别尝试了使用背景图片和 HTMLCanvasElement 对象，最后决定使用后者，通过 canvas 实现动画的好处是可以随意对图片进行剪裁、移动等处理，不需要再用图片处理工具单独处理图片了。

### label 配置 HTMLCanvasElement  
```javascript
label: {
    show: true,
    formatter: function (params) {
        return `{img|}{name|${params.name}}`
    },
    rich: {
        img: {
            backgroundColor: {
                image: canvasElement, // 图片路径
                // 这里可以是图片的 URL
                // 或者图片的 dataURI
                // 或者 HTMLImageElement 对象
                // 或者 HTMLCanvasElement 对象
            },
            width: 30,
            height: 80,
        },
        name: {
            color: '#fffcf8',
            fontSize: 14,
            verticalAlign: 'bottom',
            padding: [0, 0, 0, 1],
        },
    },
},

async function createCanvas() {
    const canvas = document.createElement('canvas')
    const ctx = canvas.getContext('2d')
    canvasElement = canvas

    let currentIndex = 0

    const motionAnimation = async () => {
        try {
            const img = await loadImage(images[currentIndex])
            ctx.clearRect(0, 0, canvas.width, canvas.height)
            ctx.drawImage(img, 90, 0, 72, 210, 0, 0, canvas.width, canvas.height)

            // 更新图片索引
            currentIndex = (currentIndex + 1) % images.length
            // 渲染到地图上的 canvas 不会主动更新，需要主动去触发更新
            myChart?.resize()
        } catch (error) {
            console.error('image loaded failed: ', error)
        }
    }

    if (!canvasTimer) {
        // 这里可以优化为 requestAnimationFrame
        canvasTimer = setInterval(motionAnimation, 5)
        await motionAnimation()
    }
}
```

### 解决 hover 和 tootip 问题
因 map3D 加入了动态效果，动态效果需要实时更新，会实时触发 ECharts 的渲染因此会产生一些额外的问题：
1. 地图区域的 hover 效果会因为动态更新而被重置
2. 地图区域的 tooltip 会因动态更新而被重置
对上述两种情况，分别进行了如下处理：
1. 取消地图的 hover 效果，通过 mouseover 事件设置当前地区的 itemStyle，鼠标移出地图再清空所有地区的 itemStyle。
2. 默认设置 tooltip 的 alwaysShowContent 为 true，让tooltip 保持始终显示，鼠标移出地图或画布再改为 false，并主动触发一次 hideTip，让 tooltip 隐藏。
```javascript
// map3D 富文本标签部分
label: {
    show: true,
    formatter: function (params) {
        return `{img|}{name|${params.name}}`
    },
    rich: {
        img: {
            backgroundColor: {
                image: canvasElement, // 图片路径
            },
            width: 30,
            height: 80,
        },
        name: {
            color: '#fffcf8',
            fontSize: 14,
            verticalAlign: 'bottom',
            padding: [0, 0, 0, 1],
        },
    },
},

// 核心原理
function bindEvent() {
    let mouseMoveCount = 0 // echartsInstance 计数器
    let ZrMouseMoveCount = 0 // ZRender 计数器

    // 清空自定义样式
    const resetMapItemStyle = (_alwaysShowContent) => {
        let hasItemStyle = false
        let data = myChart.getOption().series[0].data
        let alwaysShowContent = chartOption.tooltip[0].alwaysShowContent

        data.forEach((item) => {
            if (item.itemStyle) {
                hasItemStyle = true
                delete item.itemStyle
            }
        })

        // 做一层判断限定，防止频繁触发地图更新
        if (hasItemStyle) {
            console.log('hasItemStyle')
            myChart.setOption({
                series: [
                    {
                        data,
                    },
                ],
            })
        }

        // 做一层判断限定，防止频繁触发地图更新
        if (_alwaysShowContent != undefined && alwaysShowContent != _alwaysShowContent) {
            console.log('alwaysShowContent != _alwaysShowContent')
            myChart.setOption({
                tooltip: {
                    alwaysShowContent: _alwaysShowContent,
                },
            })
        }

        if (!_alwaysShowContent) {
            myChart.dispatchAction({
                type: 'hideTip',
            })
        }
    }

    // 鼠标移入地图时要重置两个计数器，确保逻辑正确
    myChart.on('mouseover', (params) => {
        console.log('mouseover ', params)
        mouseMoveCount = 0
        ZrMouseMoveCount = 0

        resetMapItemStyle(true)

        let data = myChart.getOption().series[0].data
        data[params.dataIndex].itemStyle = {
            color: '#4791c4',
        }
        myChart.setOption({
            series: [
                {
                    data,
                },
            ],
        })
    })

    // 鼠标移出画布需要处理一次，防止鼠标过快移出画布不能触发mousemove事件而导致无法重置自定义样式
    myChart.on('globalout', (params) => {
        console.log('globalout ', mouseMoveCount)
        mouseMoveCount = 0
        ZrMouseMoveCount = 0
        resetMapItemStyle(false)
    })

    // 只在地图上移动鼠标触发
    myChart.on('mousemove', (params) => {
        mouseMoveCount++
    })

    // 需要注意，getZr 为 ECharts 私有函数，后续有可能会修改或移除，最好不要更新版本，
    // 若要更新，需要自测一下使用了 getZr 功能的模块
    myChart.getZr().on('mousemove', (params) => {
        console.log('===========================')
        console.log('Zr mousemove ', ZrMouseMoveCount)
        console.log('mousemove ', mouseMoveCount)
        ZrMouseMoveCount++
        let count = mouseMoveCount * 2 + 2
        // 鼠标不在地图范围，清空自定义样式
        // 只要保证 ZrMouseMoveCount 大于 count 就可以确定鼠标移出地图了，此时只有 ZRender 的计数器在累加，是一定大于 count 的
        // 所以即使在地图内 mousemove 触发两次也没有关系，不会影响判断
        if (ZrMouseMoveCount > count) {
            resetMapItemStyle(false)
        }
        console.log(`ZrMouseMoveCount:${ZrMouseMoveCount} | count:${count}`)
        console.log(ZrMouseMoveCount > count ? '鼠标不在地图范围内' : '鼠标在地图范围内')
    })
}
```

### 解决动画卡顿问题
解决上述问题后，接下来碰到了优化问题，因为是逐帧动画，在初始化时候浏览器需要加载几十张图片，这会导致在渲染动画时产生卡顿感，对此需要对所有图片预加载并将图片缓存，待预加载完成之后再加载动画。
```javascript
// 预加载图片
// 将逐帧动画全部预加载后再渲染到 ECharts，不会重置 tooltip，但是切换地图后还是会有这个问题
const preLoadedImages = {}
const preLoadedImagesTasks = []

for (const imageUrl in imageList) {
    images.push(imageUrl)
    preLoadedImagesTasks.push(loadImage(imageUrl))
}

async function init() {
    // 一定要处理异常，不然图片出错阻断地图渲染
    await Promise.all(preLoadedImagesTasks).catch(err => console.log('preLoadedImagesTasks err: ', err))
    await createCanvas().catch(err => console.log('createCanvas err: ', err))
    // renderChart
}

function loadImage(src) {
    return new Promise((resolve, reject) => {
        if (preLoadedImages[src]) {
            resolve(preLoadedImages[src])
        } else {
            const img = new Image()
            img.src = src
            img.onerror = reject
            img.onload = () => {
                preLoadedImages[src] = img
                resolve(img)
            }
        }
    })
}
```

### 优化拖拽地图卡顿
大部分情况是由于事件中存在频繁触发更新图表的逻辑，比如上面提到的自定义 hover 效果，就会在 mouseover 中频繁触发图表更新，因此在拖拽过程中尽量不触发任何事件，可以避免因频繁更新图表导致的卡顿。

但是map3d拖拽事件并没有，要自己实现，通过监听地图的 mousedown 和 mouseup 事件来实现，简易方式就是鼠标按下时候认为是在拖拽，鼠标松开结束拖拽，再优化一下则是再按下鼠标时，mousemove 了一段距离后，判定为正在拖拽，鼠标松开结束拖拽。

需要注意的是，要在整个画布上监听鼠标事件，也就是 ZRender 的监听事件。
```javascript
// 简易版本
let isDraging = true

myChart.getZr().on('mousedown', (params) => {
    console.log('mousedown ', params)
    isDraging = true
})

myChart.getZr().on('mouseup', (params) => {
    console.log('mouseup ', params)
    isDraging = false
})
```

```javascript
// 优化版本
let isMouseDown = false
let isDraging = true

myChart.getZr().on('mousemove', (params) => {
    if (isMouseDown) {
        isDraging = true
    }
}

myChart.getZr().on('mousedown', (params) => {
    console.log('mousedown ', params)
    isMouseDown = true
})

myChart.getZr().on('mouseup', (params) => {
    console.log('mouseup ', params)
    isMouseDown = false
    isDraging = false
})
```

### map3D DEMO
地图json数据可以到[地图数据-阿里云DataV数据可视化][2]中去下载。
```javascript
let json // 这里添加地图数据
echarts.registerMap('3dMap', json)

myChart.setOption({
  tooltip: {
    show: true,
    backgroundColor: '#0C2257',
    borderColor: '#0C2257'
  },
  series: [
    {
      type: 'map3D',
      name: '地图',
      map: '3dMap',
      regionHeight: 3, // 地图高度
      viewControl: {
        distance: 100, // 地图视角 控制初始大小
        maxDistance: 130, // 最大距离
        alpha: 50, // 视角绕 x 轴，即上下旋转的角度
        beta: 0, // 视角绕 y 轴，即左右旋转的角度
        animation: false, // 控制 3D 地图视图在调整时是否使用动画过渡效果
        center: [5, 0, 0] // 中心
      },
      bottom: '10',
      data: [],
      itemStyle: {
        borderWidth: 0.5, // 分界线wdith
        borderColor: '#ccc', // 分界线颜色
        opacity: 1,
        color: '#5D99DF'
      },
      emphasis: {
        itemStyle: {
          color: '#4791c4 '
        }
      },
      light: {
        // 场景主光源的设置，在 globe 组件中就是太阳光
        main: {
          color: '#fff',
          intensity: 0.7, // 主光源的强度
          shadow: true,
          shadowQuality: 'low', // 阴影的质量。可选'low', 'medium', 'high', 'ultra'
          alpha: 45,
          beta: -90
        },
        // 全局的环境光设置
        ambient: {
          color: '#7DD0FF',
          intensity: 0.6
        }
      }
    }
  ]
});
```

# 参考
1. [rich使用文档][1]
2. [地图数据-阿里云DataV数据可视化][2]

[1]: https://echarts.apache.org/zh/option.html#series-bar.label.rich.%3Cstyle_name%3E
[2]: https://datav.aliyun.com/portal/school/atlas/area_selector
