---
title: Hexo Icarus
date: 2020-1-25 20:24:12
categories: 技术框架
tags: Hexo
toc: true
---

## 设置缩略图

文章头添加 cover 属性及图片路径

<!-- more -->

## 插入图片
{% raw %}
```ejs
{% img class-names /img/covers/pic_1.png 400 100 [title text [alt text]] %}
```
{% endraw %}



## 文章目录

编辑 _config.yml， 默认开启

```bash
widgets:
    -
        type: toc
        position: left #展示位置 左或右
```

在需要显示目录的文章的.md开头插入

```bash
toc: true
---

文章内容...
```



如需自定义 style，需到 hero-theme-icarus/include/style/card.styl 中自行修改。



## 代码高亮

如果你已在Hexo中启用了代码高亮功能，你可以通过`article`中的`highlight`设置来自定义代码块。 请从[highlight.js/src/styles](https://github.com/highlightjs/highlight.js/tree/9.18.1/src/styles)下列出的所有主题中 选择一个主题。 然后，复制文件名(不带`.css`后缀)到`theme`设置项中。

如要隐藏复制代码按钮，将`clipboard`设置为`false`。 如果你希望折叠或展开所有代码块，将`fold`设置为`"folded"`或`"unfolded"`。 你也可以将`fold`设置为空来禁止代码块折叠。

```ejs
_config.icarus.yml

article:
    highlight:
        # 代码高亮主题
        # https://github.com/highlightjs/highlight.js/tree/master/src/styles
        theme: atom-one-light
        # 显示复制代码按钮
        clipboard: true
        # 代码块的默认折叠状态。可以是"", "folded", "unfolded"
        fold: unfolded
```

此外，你可以在Markdown文件中使用下面的语法来折叠单独的代码块：
{% raw %}
```ejs
{% codeblock "可选文件名" lang:代码语言 >folded %}
...代码块内容...
{% endcodeblock %}
```
{% endraw %}



## 修改图片填充方式

hexo-theme-icarus/include/style/helper.styl，修改 object-fit 属性。

## 修改文中插图样式

hexo-theme-icarus/include/style/plugin.styl 中新增如下。


## 修改背景

在 hexo-theme-icarus/include/style/base.styl 中调整 style。


## 修改导航

在 hexo-theme-icarus/layout/common/navbar.jsx 中修改导航 logo，在 _config.icarus.yml 中配置 logo 属性。


## 归档调整

在 hexo-component-inferno/lib/view/widget/archives.js 中调整 _props$type 值，默认为 MMMM YYYY。


## 部署

​	Travis CI 对于开源 repository 是免费的，但是这意味着你的站点文件将会是公开的，如果将仓库设置 private，那么要自行手动部署。



## 常见问题

#### fatal: in unpopulated submodule '.deploy_git'

rm -rf .deploy_git 再进行生成和部署



## 参考

1. [ICARUS Helpers](https://ppoffice.github.io/hexo-theme-icarus/uncategorized/hexo-built-in-tag-helpers/#Image)
2. [Hexo-Icarus主题配置建议](https://blog.andycen.com/2020/03/07/Hexo-Icarus%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE%E5%BB%BA%E8%AE%AE/)
3. [Hexo 部署](https://hexo.io/zh-cn/docs/github-pages)

