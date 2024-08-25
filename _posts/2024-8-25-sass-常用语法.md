---
title: Sass 常用语法
date: 2018-12-21 21:03:01
categories: 基础技术
tags: 
  - sass
  - css
toc: true
---

	Sass 是一个将脚本解析成CSS的脚本语言，即SassScript。

Sass 有两种语法格式。首先是 SCSS (Sassy CSS) —— 也是本文示例所使用的格式 —— 这种格式仅在 CSS3 语法的基础上进行拓展，所有 CSS3 语法在 SCSS 中都是通用的，同时加入 Sass 的特色功能。此外，SCSS 也支持大多数 CSS hacks 写法以及浏览器前缀写法 (vendor-specific syntax)，以及早期的 IE 滤镜写法。这种格式以 .scss 作为拓展名。

<!-- more -->

另一种也是最早的 Sass 语法格式，被称为缩进格式 (Indented Sass) 通常简称 "Sass"，是一种简化格式。它使用 “缩进” 代替 “花括号” 表示属性属于某个选择器，用 “换行” 代替 “分号” 分隔属性，很多人认为这样做比 SCSS 更容易阅读，书写也更快速。缩进格式也可以使用 Sass 的全部功能，只是与 SCSS 相比个别地方采取了不同的表达方式。

--------

## SCSS ##
### 自定义变量 ###

```
$width: 100px;
.main {
  width: $width;
}
```

变量同时支持块级作用域，只能在嵌套规则内部使用，可以使用 !global 将局部变量转换为全局变量
```
.main {
  $width: 100px !global;
  width: $width;
}
.left {
  width: $width;
}
```

### 运算 ###

```
$width: 100px;
.main {
  width: $width/2
  height: 3px + (4px * 2);
  font-family: sans- + "serif"
}
```

### 插值语句 ###

通过使用 #{} 可以在选择器或属性名上使用变量
```
$className: main;
$attr: border;
.#{className} {
  #{attr}-color: red;
}
```

### 嵌套规则 ###

```
.main {
  width: 100px;
  p {
  	font-size: 15px;
  }
}
```

### 父选择器标识符 ###

使用 & 在伪类上
```
$width: 100px;
.main a {
  &:hover {
  	color: red;
  }	
}
```

### 导入 ###

导入 sass 文件可以省略后缀名
```
@import 'style';
```

sass 允许嵌套导入，将 @import 写在 css 规则内
```
.main { @import 'style' }
```

### 注释 ###

```
.main {
  width: 100px; // 这种注释内容不会出现在生成的css文件中
  padding: 5px; /* 这种注释内容会出现在生成的css文件中 */
}
```

### 混合器 ###

```
@mixin rounds {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
.main p {
  @include rounds;	
}
```

传参
```
@mixin rounds($value) {
  -moz-border-radius: $value;
  -webkit-border-radius: $value;
  border-radius: $value;
}
.main p {
  @include rounds(5px);	
}
```

默认参数
```
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

### 继承 ###

```
.rounds {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
.main p {
  @extend .rounds;	
}
```

如果不希望继承的属性单独出现在 CSS 中，使用 % 进行声明
```
%rounds {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
.main p {
  @extend %rounds;	
}
```

### 判断语句 ###

```
.main {
  @if 3 > 2 {
  	width: 100px;
  }	@else {
  	width: 50px;
  }
}
```

### 循环 ###

```

//through 与 to 的含义：
//当使用 through 时，条件范围包含 <start> 与 <end> 的值
//而使用 to 时条件范围只包含 <start> 的值不包含 <end> 的值
//$var 可以是任何变量，比如 $i
//<start> 和 <end> 必须是整数值

@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}

@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

### 函数 ###

```
@function getValue ($value) {
  @return $value/750;
}
.main {
  width: getValue(350px);
}
```

## 参考文献 ##

[Sass 官方文档](https://www.sass.hk/)