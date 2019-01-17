---
title: Vue 使用总结
date: 2017-09-24 19:43:31
tags: Vue
categories: Framework
---

## 列表跳详情 ##

### 路由传参 ###
> 除了使用 <router-link> 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。
> 在 Vue 实例内部，你可以通过 $router 访问路由实例，因此你可以调用 this.$router.push。

<!-- more -->
> 一个路由对象 (route object) 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的路由记录 (route records)。
> 路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象,在组件内部，即 this.$route。

1.params 传参
```
/** 传参 **/
//html
<router-link v-for="(item,index) in listData" :to="{name: 'detail',params:{id: item.id}}" >

//js
this.$router.push({
  name: 'detail',
  params: {
    id: id
  }
})


/** 取参 **/
//html
<div> 
  {{ $route.params.id }}
</div>

//js
this.$route.params.id
```

2.query 传参
```
/** 传参 **/
//html
<router-link v-for="(item, index) in listData" tag="div" :to="{path: '/detail', query: {id: item.id}}">{{item.id}}</router-link>

//js
this.$router.push({
  path: '/detail',
  query: {
    id: id
  }
})


/** 取参 **/
//html
<div>
  {{ $route.query.id }}
</div>

//js
this.$route.query.id
```

3.路径传参
配置路由
```
{
  path: '/detail/:id',
  name: 'detail',
  component: detail
}
```
使用
```
/** 传参 **/
//html
<router-link v-for="(item, index) in listData" :to="{path: 'detail/'+ item.id}">{{item.id}}</router-link>

//js
this.$router.push({
  path: `/detail/${id}`
})


/** 取参 **/
this.$route.params.id
```

### 跳转位置 ###
在列表页跳转到详情页的时候，当列表页发生滚动的时候，跳转的详情页也会相应发生位置的变更，这就很不友好了，可以使用
```
$(window).scrollTop(0);
```

但其实在 vue-router 中，也已经提供了更详细的 scrollBehavior 方法，在创建 router 实例阶段，添加这个方法：
```
scrollBehavior （to, from , savedPosition）{
    if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```

savedPosition参数当且仅当 popstate（通过浏览器的前进后退按钮触发）时才有用，返回之前的位置 。


> vue-router 规定，在使用 scrollBehavior 方法时，必须要在 H5 的 history 模式下才可以使用，然而这就面临一个问题，在服务器端访问的时候，由于从 hash 访问变为 history 模式访问，路由中配置的根路径会直接指向到服务器的根目录，而不是项目本身所在的路径，这个坑又是困扰我了好久，虽然官方文档给出了解决方案，就是修改服务器端的配置文件: 在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

我的服务器是nginx，用了这个方法重启服务器之后，发现问题依然没有解决（具体我也不知道为什么），于是继续向度娘求助，有人说可以直接使用绝对路径，就是在路由配置中将相对路径直接修改为绝对路径，试了这个办法，果然可行，可是这么做本地线上不同步，而且只要是有路由的地方都要修改添加，很耽误开发效率。无意间，看到了一位大神的回答，在路由实例化阶段，有一个 base 属性（官方文档并没有给出），表示应用的基路径，默认为根路径，所以只要修改这个属性值就不再需要进行多余的操作了。


## 返回 ##
使用 History 对象的 history.go(-1)，也可以通过 router 的实例方法，使用方法和 window.history 基本一致：
```
mechods:{
  goBack(){
    this.$router.go(-1);
  }
}
```

## 回到顶部 ##
```
<template>
  <div class="back-to-top" v-show="backTop">
    <a href="#"><i class="fa fa-angle-up"></i></a>
  </div>
</template>
<script>
  export default {
    name: "backToTop"
    data(){
      return {
        backTop: false
      }
    },
    methods: {
      handleScroll(){
        if(window.scrollY > 500){
          this.backTop = true;
        } else {
          this.backTop = false;
        }
      }
    },
    mounted(){
      window.addEventListener(''scroll'',this.handleScroll,false);
    }
  }
</script>
<style lang="less">
.back-to-top {
  position: fixed;
  bottom: 10rem;
  right: 2rem;
  padding: 0 .8rem;
  border-radius: .4rem;
  background: rgba(0,0,0,0.7);
  font-size: 3.2rem;
  i{
    color: #fff;
  }
}
</style>
```  
