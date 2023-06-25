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

#### Code Runner
Code Runner 是一个在 Visual Studio Code 编辑器中使用的扩展插件，它提供了一个快速、方便的方式来运行和调试代码片段。
Code Runner 插件的主要功能如下：
- 支持多种编程语言：Code Runner 支持许多常见的编程语言，包括 JavaScript、Python、Java、C++、C#、Go、Ruby 等。你可以在编辑器中直接运行这些语言的代码片段。
- 快速运行：通过单击或使用快捷键，你可以快速运行当前文件或选定的代码块。你不需要离开编辑器或打开终端来运行代码。
- 输出结果：Code Runner 可以将代码的运行结果输出到“输出”面板，可以在其中查看程序的输出、错误信息和调试信息等。
- 代码片段调试：除了运行代码，Code Runner 还支持调试代码片段。你可以设置断点、逐步执行代码并查看变量的值，帮助你进行代码调试。
- 自定义命令和参数：该插件允许你自定义运行命令和参数，以满足特定项目或语言的需求。你可以根据自己的偏好配置运行环境。
- 支持代码模板：你可以为每种语言设置代码模板，简化代码编写过程。每次创建新文件或代码块时，插件会自动插入代码模板。
总的来说，Code Runner 是一个非常实用的插件，它使得在 Visual Studio Code 中运行和调试代码更加方便和高效。你可以快速尝试不同编程语言的代码片段，并查看输出结果或进行代码调试，而无需切换到其他工具或终端窗口。

#### Color Highlight
颜色高亮

#### Color Picker
颜色拾取器

#### Vue Language Features（Volar）
Vue Language Features（Volar）是针对 Vue.js 开发的 Visual Studio Code 编辑器的扩展插件，它提供了一系列强大的特性，帮助开发者更好地编写和调试 Vue.js 代码。
Vue Language Features（Volar）插件的主要功能如下：
- 智能提示：插件提供了全面的智能提示功能，包括组件、指令、选项、属性、方法等。当你在编写 Vue.js 代码时，插件会显示相关的自动补全建议，帮助你快速输入正确的代码。
- 模板验证：该插件可以验证 Vue 模板中的语法错误和潜在问题，并在编辑器中进行实时提示。这有助于提早发现并修复代码错误，提高代码质量。
- 快速导航：插件支持快速导航到 Vue 文件中的组件、引入的模块和定义的变量等。你可以通过点击或使用快捷键，在不同文件之间方便地跳转和浏览代码。
- 代码重构：插件提供了重构功能，可以帮助你快速重命名组件、变量和函数等，并自动更新所有相关的代码引用。这样可以减少手动修改的工作量，确保代码的一致性。
- 完整的类型推断：通过对 Vue 组件和组件选项的类型推断，插件可以提供准确的类型检查和验证。这有助于捕获潜在的类型错误和编码问题。
- 即时文档：插件内置了 Vue.js 的文档，可以在编辑器中直接查看 Vue 指令、组件选项的说明和用法示例。这样你就可以快速查阅相关文档，减少在浏览器中搜索的时间。
总的来说，Vue Language Features（Volar）是一个功能强大的 Visual Studio Code 插件，为 Vue.js 开发者提供了丰富的功能和工具，帮助提高开发效率、减少错误，并提供更好的开发体验。无论是编写 Vue 组件、调试代码还是查阅文档，该插件都能为你提供很大的帮助。

#### vue3-snippets-for-vscode
一个适配 Vue3 Api 的 snippets 插件

#### ES7+ React/Redux/React-Native snippets
vscode-react-javascript-snippets 是针对 React 和 JavaScript 开发的 Visual Studio Code 编辑器的扩展插件，它提供了一套丰富的代码片段，帮助开发者更快速、高效地编写 React 和 JavaScript 代码。
vscode-react-javascript-snippets 插件的主要功能如下：
- React 代码片段：该插件提供了大量与 React 相关的代码片段，包括组件的创建、生命周期方法、事件处理、状态管理等。你可以使用这些代码片段来加速 React 应用程序的开发。
- JavaScript 代码片段：除了 React，插件还包含了许多常用的 JavaScript 代码片段，涵盖了变量声明、循环、条件判断、函数定义等常见的编码模式。这些代码片段可以提高你在 JavaScript 开发中的效率。
- 快捷键触发：通过输入特定的触发词（trigger word）并按下 Tab 键，你可以快速插入对应的代码片段。这样可以减少手动编写模板代码的工作量。
- 智能补全：当你输入触发词时，插件会显示相应的代码片段建议。你可以从建议列表中选择并填充代码片段。这可以防止拼写错误和提供代码语法的提示。
- 支持自定义：如果你不满足于默认提供的代码片段，插件还允许你自定义和定制自己的代码片段。你可以根据项目需求或个人偏好进行修改。
vscode-react-javascript-snippets 提供了一组强大而便捷的代码片段，帮助你在 React 和 JavaScript 开发中快速生成常用的代码结构和模式。使用这些代码片段可以加速开发过程，提高代码编写效率，减少重复劳动。

#### Open In Default Browser
在资源管理器和编辑器里的html、xml、pdf文件提供“在浏览器中打开”菜单，它可以使用默认浏览器打开此文件

#### Prettier
Prettier 是一个流行的代码格式化工具，而 Prettier 插件是 Visual Studio Code 编辑器的一个扩展，它集成了 Prettier 工具，并为开发者提供了方便的格式化代码的功能。
Prettier 插件的主要功能如下：
- 代码格式化：Prettier 插件可以根据预定义的规则自动格式化代码，使其符合统一的代码风格。它支持多种编程语言，包括 JavaScript、TypeScript、CSS、HTML、JSON 等。
- 自动保存格式化：你可以配置 Prettier 插件在保存文件时自动进行代码格式化，确保代码的风格始终保持一致。这可以节省手动运行格式化命令的时间和精力。
- 命令格式化：除了自动保存外，你还可以通过右键菜单或快捷键手动触发 Prettier 的代码格式化命令，对当前文件或选定的代码进行格式化。
- 配置定制：Prettier 插件允许你根据项目需求或个人偏好进行配置定制。你可以定义自己的代码格式化规则，包括缩进大小、分号风格、引号类型等。将配置与项目中的 .prettierrc 文件结合使用，以实现不同项目的独立配置。
- 集成编辑器功能：Prettier 插件与 Visual Studio Code 编辑器完美集成，它会与其他扩展和编辑器功能无缝协作。例如，在使用 Git 版本控制时，Prettier 可以与 Git 的 pre-commit 钩子一起使用，确保代码提交之前自动格式化。
总的来说，Prettier 插件是一款强大、方便的代码格式化工具，能够帮助开发者在 Visual Studio Code 中轻松地进行代码格式化。使用该插件可以提高代码的可读性、一致性和协作性，使团队成员在编写代码时更加专注于逻辑而不是格式。

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
