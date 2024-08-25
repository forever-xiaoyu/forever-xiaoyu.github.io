---
title: Grunt 小结
date: 2018-08-01 15:03:17
tags: Grunt
categories: 常用工具
toc: true
---

    grunt 运用配置的思想来写打包脚本，一切皆配置，所以会出现比较多的配置项，诸如 option, src, dest 等等。而且不同的插件可能会有自己扩展字段，导致认知成本的提高，运用的时候要搞懂各种插件的配置规则。
<!-- more -->
---------

## package.json
此文件被npm用于存储项目的元数据，以便此项目发布为npm模块。你可以在此文件中列出项目依赖的grunt和Grunt插件，放置于devDependencies配置段内。

```
{
  "name": "grunt",
  "version": "1.0.0",
  "description": "test",
  "author": "xiaoyu",
  "license": "ISC",
  "devDependencies": {
    "grunt": "^1.0.3",
    "grunt-autoprefixer": "^3.0.4",
    "grunt-cmd-transport": "^0.1.1",
    "grunt-contrib-clean": "^1.0.0",
    "grunt-contrib-concat": "^1.0.1",
    "grunt-contrib-connect": "^2.0.0",
    "grunt-contrib-copy": "^1.0.0",
    "grunt-contrib-cssmin": "^0.10.0",
    "grunt-contrib-htmlmin": "^3.0.0",
    "grunt-contrib-imagemin": "^1.0.1",
    "grunt-contrib-jshint": "^1.1.0",
    "grunt-contrib-sass": "^1.0.0",
    "grunt-contrib-uglify": "^2.0.0",
    "grunt-contrib-watch": "^1.1.0",
    "grunt-copy-to": "0.0.12",
    "grunt-filerev": "^0.1.2",
    "grunt-filerev-replace": "^0.1.5",
    "grunt-text-replace": "^0.4.0",
  }
}

```


## Gruntfile
此文件被命名为 Gruntfile.js 或 Gruntfile.coffee，用来配置或定义任务（task）并加载Grunt插件的。

```
module.exports = function(grunt){

    //初始化grunt 配置
    grunt.initConfig({
       //获取package.json的信息
       pkg: grunt.file.readJSON('package.json'),

        //清除临时目录
        clean: {
            build: ['temp', 'build']
        },

        //拷贝源文件
        copy: {
            toBuild: {
                expand: true,      //启用扩展选项
                // flatten: true,  //移除所有路径成分
                // ext: '.flat',   //替换扩展名
                // cwd: './',      //所有路径相对于指定的此路径
                src: ['examples/*.js'],
                dest: 'temp/'
            },
            html: {
                expand: true,      //启用扩展选项
                src: ['examples/*.html'],
                dest: 'build/',
                flatten: true
            }
        },

        //合并
        concat: {
            js: {
                src: ['examples/grunt.js', 'examples/grunt1.js'],
                dest: 'build/grunt-concat.js'
            }
        },

        //sass
        sass: {
            dist: {
                files: {
                    'examples/css/sass.css': 'examples/test.scss'
                }
            }
        },

        //autoprefixer
        autoprefixer:{
            options:{
                browsers: [
                    'ie >= 8',
                    'ff >= 10',
                    'chrome >= 20',
                    'safari >= 7',
                    'opera >= 10',
                    'ios >= 7',
                    'android >= 2.3'
                ],
                map: true
            },
            files: {
                src: 'examples/test.css',//需要加前缀的css文件
                dest: 'build/main.css'//grunt处理后生成的css文件，如果文件夹中没有该文件，则自动创建
            },
        },


        //压缩
        uglify: {
            options: {
                banner: '/*! <%= pkg.name %> created by <%= pkg.author %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
            },
            build: {
                src: 'examples/<%= pkg.name %>.js',
                dest: 'build/<%= pkg.name %>.min.js'
            }
        },

        cssmin: {
            build: {
                expand: true,
                src: 'examples/grunt.css',
                dest: 'build/',
                flatten: true,
                ext: '.min.css'
            }
        },

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
                    {
                        expand: true,
                        flatten: true,
                        src: ['examples/*.html'],
                        dest: 'build/',
                        ext: '.html'
                    }
                ]
            },
        },

        //服务
        connect: {
            server: {
                options: {
                    // protocol: 'https',   //网络通信协议
                    port: 8080,
                    base: './',
                    keepalive: false,   //让服务器持续运行。请注意，如果启用此选项，则此任务之后指定的任何任务将永远不会运行
                    livereload: 35799,
                    hostname: '*'   //使服务器可以从任何本地局域网访问 127.0.0.1
                }
            }
        },

        //监听
        watch: {
            options: {
                reload: true,
                livereload: 35799,
            },
            start: {
                files: ['examples/*.html'],
                tasks: ['copy:html'],
                options: {
                    spawn: false    //动态修改配置
                }
            }
        }

    //加载能够提供 uglify 任务的插件
    grunt.loadNpmTasks('grunt-contrib-copy');
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-cssmin');
    grunt.loadNpmTasks('grunt-autoprefixer');
    grunt.loadNpmTasks('grunt-contrib-htmlmin');
    grunt.loadNpmTasks('grunt-contrib-connect');
    grunt.loadNpmTasks('grunt-contrib-watch');

    grunt.registerTask('default', ['autoprefixer']);
};
```

## Grunt 注意问题
1. 在配置 sass 的时候，如下配置：
```
sass: {
    dist: {
        files: {
            expand: true,
            cwd: 'scss',
            src: ['*.scss'],
            dest: 'dist/css',
            ext: '.css'
        }
    }
}，
watch: {
    scripts: {
        files: ['scss/*.scss'],
        tasks: ['sass'],
        options: {
            spawn: false
        }
    }
}
```
此时报了 pattern.indexOf is not a function 的错误，是因为此时 files 为 Object，而 Object 是没有 indexOf 方法的，所以要注意 files 一定要用数组。

<div align="center">![universe](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1563339494,4060447026&fm=27&gp=0.jpg "universe")