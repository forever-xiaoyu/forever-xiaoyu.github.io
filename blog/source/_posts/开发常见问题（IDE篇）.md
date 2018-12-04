---
title: 开发常见问题（IDE篇）
date: 2018-10-26 18:52:29
categories: Others
tags: IDE
---

    在日常开发中，时常会碰到各种各样的坑，本文总结了开发过程中碰到的常见问题。

----------

## WS ##
### css 预编译配置
以 scss 为例：

```
npm install sass -g
```
<!-- more -->
在 Setting -> Tools -> File Watchers 进行如下配置：<br>
1. Program：C:\Users\yuce\AppData\Roaming\npm\sass.cmd
2. Arguments：$FileName$:$FileNameWithoutExtension$.css
3. Output：$FileNameWithoutExtension$.css:$FileNameWithoutExtension$.css.map

sourcemap: 在生产环境中，以 scss 为例，我希望看到的未经编译文件的调试信息，而非编译后的，这样会方便开发人员调试，快速定位问题，生成的 source map,  同时需要在 chrome 中开启才能生效。

--------------

### nodejs 环境配置
打开 setting 搜索 node.js 
配置 Node interpreter
勾选 code assistance for node.js

<div align=center>![ws配置nodejs环境.png](开发常见问题（IDE篇）/nodejs.png)

最后重启 ws 即可。

----------------