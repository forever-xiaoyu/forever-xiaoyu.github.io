---
title: The principle of Promise
date: 2018-08-24 09:36:45
tags: 
- Promise 
- es6
categories: 
---

```
function _Promise(callback) {
	var thenFn = [];
	var catchFn = [];

	this.then = function(fn) {
		thenFn.push(fn);
		return this;
	}

	this.catch = function(fn) {
		catchFn.push(fn);
		return this;
	}

	function resolve(res) {
		for(var i = 0; i < thenFn.length; i++) {
			thenFn[i](res);
		}
	}

	function reject(res) {
		for(var i = 0; i < catchFn.length; i++) {
			catchFn[i](res);
		}
	}

	callback(resolve, reject);
}

var Task = new _Promise(function(resolve, reject){
	setTimeout(function(){
// 		resolve("task has resolved ~");
		reject("task has catched ~");
	}, 1500)
})

Task
.then(function(res){
	console.log(res);
})
.catch(function(res){
	console.log(res)
})

```