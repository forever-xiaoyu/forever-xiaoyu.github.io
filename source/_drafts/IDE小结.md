---
title: IDE 篇
date: 2018-10-26 18:52:29
categories: 常用工具
tags: IDE
toc: true
---

    在日常开发中，时常会碰到各种各样的坑，本文总结了开发过程中碰到的常见问题。

----------

## VSCode ##
### 插件篇

#### Gitlens

#### Color Highlight

#### Git History

#### Volar

#### vue3-snippets-for-vscode

#### luzhenqian.juejin
可以在vscode中浏览掘金

#### shalldie.background
安装完成后，在setting.json中进行相应配置，详细可以看在插件详情中查看使用文档

部分配置：
```json
{
  "background.enabled": true, // 开关
  // 设置全屏背景图片
  // "background.fullscreen": {
      //   "image": "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fbkimg.cdn.bcebos.com%2Fpic%2Ff603918fa0ec08fa55410a5859ee3d6d54fbda8f&refer=http%3A%2F%2Fbkimg.cdn.bcebos.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1670039224&t=f44f8815ca858834b87a534df461a6e1", // 图片的url
  //   "opacity": 0.88, // 建议值 0.85 ~ 0.95
  //   "size": "cover", // css, 建议使用 `cover`自适应，或者 `contain`、`200px 200px`
  // },
  // 自定义图片及样式
  // "background.useDefault": false, // 是否使用默认图片
  // "background.customImages": [
  //   "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fbkimg.cdn.bcebos.com%2Fpic%2Ff603918fa0ec08fa55410a5859ee3d6d54fbda8f&refer=http%3A%2F%2Fbkimg.cdn.bcebos.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1670039224&t=f44f8815ca858834b87a534df461a6e1"
  // ],
  "background.style": {
    // "content": "''",
    // "pointer-events": "none",
    // "position": "absolute",
    // "z-index": "99999",
    // "width": "100%",
    "background-position": "98% 100%",
    // "background-repeat": "no-repeat",
    // "background-size": "36vw",
    "opacity": 0.4
  }
}
```

#### background
可以自定义背景

#### z-reader
支持本地、在线浏览小说、文本等内容

#### fisher

#### 我不爱掘金
嵌入 vscode 中的掘金
