---
title: grunt html 压缩简单配置
date: 2018-08-01 15:03:17
tags: grunt
categories: fe
---

** html 压缩插件 **

这里需要用到 grunt-contrib-htmlmin 

---------

## 具体配置

```
module.exports = function(grunt){

    //初始化grunt 配置
    grunt.initConfig({
       //获取package.json的信息
       pkg: grunt.file.readJSON('package.json'),
        
       //压缩HTML
       htmlmin: {
         options: {
           removeComments: true,//移除注释
           removeCommentsFromCDATA: true,//移除<script>和<style>标签内的HTML注释
           collapseWhitespace: true,//移除文档树中文本节点的空白。不会影响重大的空白，比如在SCRIPT,STYLE,PRE或TEXTAREA等元素内容
           customAttrSurround: [],//允许支持自定义属性环绕变大时的正则表达式数组（例如<input {{#if value}}checked="checked"{{/if}}>）
           customAttrAssign: true,//允许支持自定义属性赋值表达式的正则表达式数组（例如'<div flex?="{{mode != cover}}"></div>'）
           customEventAttributes: true,//允许支持自定义事件属性的正则表达式数组minifyJS（例如ng-click）
           collapseBooleanAttributes: true,//删除Boolean属性
           removeAttributeQuotes: true,//删除属性的引号，当安全的情况下
           removeRedundantAttributes: true,//删除多余的属性，像type="text/javascript"
           useShortDoctype: true,//用短的HTML5的<!DOCTYPE html>代替doctype
           processScripts: ['text/javascript'],//对应于类型脚本的元素串的阵列通过minifier处理（例如text/ng-template，text/x-handlebars-template等）
           removeEmptyAttributes: true,//删除空（或空白）属性
           removeOptionalTags: true,//一些元素允许省略标签，像</td>
           ignoreCustomFragments: [ /\{\{[\s\S]*?\}\}/ ]//忽略定制的片段
         },
         html: {
           files: [
             {expand: true, cwd: 'html', src: ['*.html'], dest: 'dist'}
           ]
         }
       },

       //watch自动化
       watch:{
           build:{
               files:['html/*.html'],
               tasks: ['htmlmin'],
               options:{spawn:false}
            }
        }
    });

    //告诉grunt我们将使用插件
    grunt.loadNpmTasks('grunt-contrib-htmlmin');
    grunt.loadNpmTasks('grunt-contrib-watch');
    grunt.registerInitTask('default',['htmlmin','watch']);
};
```

![universe](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1563339494,4060447026&fm=27&gp=0.jpg "universe")