---
title: 聊一聊 call、apply 与 bind
date: 2019-07-02 19:51:23
cover: /img/covers/call-apply.png
tags: JavaScript
categories: 基础技术
toc: true
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
  // 若调用者非函数，return 或抛出异常
  if (typeof this !== 'function') {
  	return
  }

  // 保存原始调用函数
  let self = this

  // 保存通过 bind 传入的参数
  let args = Array.prototype.slice.call(arguments, 1)

  // 定义 bind 返回的函数
  let fbind = function () {
 
    // 若是直接使用 fbind 函数，则可以直接绑定 _this 即可
    // 如果通过 new 创建 fbind 函数，此时需要将上下文绑定到新生成的实例上，即 this
    // 若是 new 创建的实例，那么此时上下文则是指向 fbind 函数
    // 因此可以通过 this instanceof fbind 来判断是 new 创建的实例还是直接调用

    // 除此之外，调用 find 函数时传入新的参数，需要将 bind 传入的参数与 fbind 传入的参数进行合并
    return self.apply(
      this instanceof fbind ? this : _this,
      args.concat(Array.prototype.slice.call(arguments))
      )
  }


  // 返回一个新的 fbind 函数
  return fbind
}
```

至此，一个 bind 函数就完成了。 再看下一个例子：

```javascript
let obj = {
    msg: 'msg',
    func: function () {
        console.log(this.msg)
    }
}

let func1 = obj.func
let func2 = obj.func.bind(obj)

func1() // undefined
func2() // msg
```

这里注意到 func1() 调用的时候输出了 undefined，是因为 func 是通过函数名传递并执行的，此时方法内部的 this 会丢失（同理，通过传入回调函数再调用也会出现同样的问题），指向 window（严格模式下为 undefined）。所以 func2 在调用时为其绑定了上下文至 obj，此时无论如何调用，函数内部的 this 都永远指向 obj。