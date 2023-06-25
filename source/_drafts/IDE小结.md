---
title: IDE 篇
date: 2018-10-26 18:52:29
categories: 常用工具
tags: IDE
toc: true
---

    在日常开发中，时常会碰到各种各样的坑，本文总结了使用 IDE 过程中常见的问题以及一些使用经验。

----------

## VSCode ##
### 插件篇

#### Code Runner

#### GitLens
GitLens 是一个在 Visual Studio Code 编辑器中使用的强大的 Git 特性扩展插件。它提供了许多有用的功能，帮助开发者更好地理解和管理代码库中的 Git 历史记录。
以下是 GitLens 插件的一些主要功能：
- 行级别的 Git 注解：GitLens 可以在每行代码上显示 Git 提交的详细信息，包括作者、提交时间和提交消息等。这有助于你了解每行代码的修改历史。
- 代码镜像：GitLens 允许你通过鼠标悬停或单击查看任意代码块的 Git 提交历史，包括该块的最近修改和引入的更改等。这对于追踪代码变更非常有用。
- 分支和标签可视化：你可以轻松地查看当前文件的分支和标签信息，并且可以直接从编辑器中切换到其他分支或标签。
- 代码演化视图：GitLens 提供了一个以时间为轴的可视化界面，展示了文件的修改历史。你可以快速查看每次提交的更改，并导航到相应的补丁或提交信息。
- 代码作者和贡献者统计：通过 GitLens，你可以查看每个文件的代码作者和贡献者统计信息，包括代码提交数量和时间线等。
- 与 Git 相关命令的快捷方式：GitLens 允许你在编辑器中执行常见的 Git 操作，如提交、拉取、推送和检出等，而无需离开编辑器。
总体而言，GitLens 提供了丰富的 Git 功能和可视化工具，使得在 Visual Studio Code 中使用 Git 更加方便和高效。它能够帮助开发者更好地理解代码的演化和历史，并提供了一些便捷的操作来进行版本控制和协作。

#### Git History
Git History 是一款用于 Visual Studio Code 编辑器的插件，它提供了强大的 Git 历史记录可视化和操作工具。
Git History 插件的主要功能如下：
- Git 历史记录可视化：Git History 允许你在 Visual Studio Code 中以图形化方式查看代码库的 Git 历史记录。你可以浏览提交树、分支、标签等信息，以及每次提交的详细更改。
- 代码文件比较：通过 Git History，你可以比较不同提交之间的代码文件差异。它显示修改的行、添加的行和删除的行，帮助你更好地了解代码的演变和变更。
- 交互式重演提交：该插件支持交互式地重演提交。你可以选择一个或多个提交，并在编辑器中查看其更改。这对于回顾和审查特定提交的更改非常有用。
- 文件历史记录：Git History 提供了文件级别的历史记录功能。你可以查看文件的每个版本，并比较不同版本之间的更改。
- 快捷键和命令：该插件为你提供了一组快捷键和命令，使你能够方便地执行常见的 Git 操作，如提交、重置、拉取等。
Git History 插件提供了直观易用的界面，帮助开发者更好地理解和管理代码库的 Git 历史记录。它使得查看代码演变、比较文件差异以及进行提交相关的操作变得更加便捷和可视化。

#### Color Highlight
颜色高亮

#### Color Picker
颜色拾取器

#### Vue Language Features（Volar）
一款用于VS Code编辑器的Vue.js开发插件

#### vue3-snippets-for-vscode
一个适配 Vue3 Api 的 snippets 插件

#### ES7+ React/Redux/React-Native snippets

#### Open In Default Browser
从浏览器打开指定文件

#### Prettier
代码格式化神器

#### ms-ceintl.vscode-language-pack-zh-hans
适用于 VS Code 的中文（简体）语言包
可在官网查看详细使用：[使用文档](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)

#### luzhenqian.juejin
可以在vscode中浏览掘金

#### shalldie.background
一个可以设置编辑器背景的插件
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

#### z-reader
支持本地、在线浏览小说、文本等内容

#### 我不爱掘金
嵌入 vscode 中的掘金

#### ChatGPT - EasyCode
使用下来感觉不够友好，经常出错没有响应，不推荐使用
