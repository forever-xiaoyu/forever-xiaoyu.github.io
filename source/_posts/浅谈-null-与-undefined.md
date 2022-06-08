---
title: 浅谈 null 与 undefined
date: 2017-8-13 16:19:33
tags: JavaScript
categories: 基础技术
toc: true
---

## undefined类型

undefined类型只有一个值，即特殊的undefined。在声明变量但未对其加以初始化时，这个变量的值就是undefined。如：
<!-- more -->
```
var name;

console.log（name）;    //undefined
```

上面例子只是声明了变量 name， 但未对其进行初始化。比较这个变量与 undefined 字面量，结果是相等的：
```
 var name = undefined;

console.log（name）;
```
 但是，有一点需要注意，包含 undefined 值得变量与尚未定义的变量还是不一样的。如：
```
var name;

console.log（name）;    //undefined

console.log（name）;    //报错
```
 对于未声明过的变量，只能执行一种操作，就是使用typeof操作符检测其数据类型。无论是未初始化的变量还是未声明的变量执行typeof操作符都会返回undefined值。其实这个结果有其逻辑上的合理性。因为虽然这两种变量从技术角度看有本质区别，但是实际上无论哪种变量都不可能执行真正的操作。

## null类型

null类型是第二个只有一个值得数据类型，这个特殊的值就是null。从逻辑角度上来看，null值便是一个空对象指针，这也是使用typeof操作符检测null值会返回object的原因。如：
```
var obj = null;

console.log(typeof(obj));    //object
```
一般来说，如果定义的变量准备在将来保存对象，那么最好将该变量初始化为null而不是其它值。这样一来，只要直接检查null值是否已经保存了一个对象的引用，如：
```
if(obj != null){

    //code

} 
```
实际上，undefined值是派生自null值的，因此ECMA规定对它们的相等性测试要返回true：
```
console.log(null == undefined);    //true 
```
这个操作符（==） 处于比较会转换其操作数（null和undefined会返回true，因为它们是类似的值，但是全等于返回false，因为它们是不同类型的值）。尽管null和undefined有这样的关系，但是他们用途完全不同。无论什么时候都没有必要把一个变量值显式的设为undefined，可同样的规则却不适用于null。简单来说，只要意在保存对象的变量还没有真正保存对象，就应该明确让变量保存null值。这样做不仅可以体现null作为空对象指针的惯例，而且也有助于进一步区分null和undefined。

 