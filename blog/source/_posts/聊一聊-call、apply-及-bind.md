---
title: 聊一聊 call、apply 及 bind
date: 2019-07-02 19:51:23
tags: JavaScript
categories: JavaScript
---

> call 和 apply 这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。

<!-- more -->

## apply
apply() 方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组，其中第二个参数可以是 Array 的实例，也可以是 arguments 的对象。

```javascript
function sum(num1, num2) {
	return num1 + num2
}

function applynum1(num1, num2) {
	return sum.apply(this, arguments)
}

function applynum2(num1, num2) {
	return sum.apply(this, [num1, num2])
}

applynum1(10, 10) // 20
applynum2(10, 10) // 20
```

上面例子中，applynum1() 和 applynum2() 执行时传入了 this 作为 this 值，因为是全局作用域中调用的，所以 this 指向 window 对象。

---------
<strong>
	在严格模式下，未指定环境对象调用函数，this 不会指向 window。除非明确把函数添加到某个对象或者调用 apply() 或 call()，否则 this 值将是 undefined。
</strong>

## call
call() 方法与 apply() 方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 call() 方法而言，第一个参数 this 值没有变化，变化的是其余参数都直接传递给函数。换句话说，在使用 call() 方法时，传递给函数的参数必须逐个列举出来。

对于 call() 和 apply() 来说，它们真正的作用是用来扩充函数赖以运行的作用域。例如，我们经常用到的判断引用类型：

```javascript
Object.prototype.toString.call([])
// [object Array]

Object.prototype.toString.call(new Date())
// [object Date]
```

这里我们借用了 Object 的 prototype 上的 toString 方法，修改了函数运行的作用域，this 分别指向了传入的 [] 和 new Date()，进而实现了判断引用类型的方法。

## bind
bind() 方法会创建一个函数的实例，其 this 值会被绑定到传给 bind() 函数的值。

对于 IE9 以下版本的浏览器，是不支持 bind() 方法的，我们可以手写一个 bind 函数来使用：

```javascript
Function.prototype.bind = function (_this) {
  if (typeof this !== 'function') {
  	return
  }

  let self = this
  let args = Array.prototype.slice.call(arguments, 1)

  let fbind = function () {
 
    return self.apply(
      this instanceof fbound ? this : _this,
      args.concat(Array.prototype.slice.call(arguments))
      )
  }

  return fbind
}
```

至此，一个 bind 函数就完成了。