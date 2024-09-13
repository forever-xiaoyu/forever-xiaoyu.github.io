---
title: npm 发包
date: 2024-09-13 20:00:38 +0800
tags: npm
categories: 技术框架
---

## 发布流程
### 配置 package.json
默认情况下，vite build 使用 production 模式，要构建库，需要显式配置 build.lib 并且可以根据需求进行调整。

mode 是控制 Vite 构建过程的环境选项，不同的模式会影响构建优化和输出结果。
```json
{
  "name": "@org/api",
  "version": "0.0.7",
  "description": "this is a description",
  "main": "dist/org-api.mjs",
  "script": "vite build --mode lib"
}
```

### 打包配置
这里以vite为例
```javascript
build: {
  lib: {
    entry: './src/index.ts', // 入口文件
    name: 'org/api',
    fileName: 'org-api',
    formats: ['es'], // 指定构建产物的格式 es cjs umd iife，也可以同时指定多个
  },
  rollupOptions: {
    external: ['vue'],
    output: {
      globals: {
        vue: 'Vue',
      },
    },
  },
}
```
配置好以后，就可以开始发布流程了，若是需要发布范围包，需要到 npm 站点添加 origanigation，这样就可以发布带有组织名称的包了（如 @vue/share）。

### 发布
执行下面命令，发布公共包，若是直接运行可能会 Code E402，默认发布范围包（如 @vue/core）会发布私有包，私有包是收费服务。
```bash
npm run build
npm publish --access public
```

## 常见问题
