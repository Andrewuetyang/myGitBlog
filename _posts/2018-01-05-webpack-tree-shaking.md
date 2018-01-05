---
layout: single
title: webpack2及以上的tree-shaking功能探索
date: 2018-01-05 21:00 +0800
category: front-end
---

## 从很容易就会思考的一个问题开始

以下代码是es6语法关于模块导入的，当我们使用webpack打包的时候，很自然会想到这个打包过程是将整个模块打包进来还是我要用的，即引入的部分打包进来呢？

```
import { a } from './module'

a()
```
在webpack1的时候，这个打包过程是将整个模块打包进来，不管你这个文件使用了模块多少的代码。显然，这是不合理的，理想情况，我们更愿意见到使用的部分打包进来，其余部分最好给我们清理掉，打包一堆没用的东西进来，占用了bundle的体积。据说最开始，rollup.js(前端打包工具，跟webpack是同类型产品)，实现了这个功能，并起了一个非常形象的名词，叫tree-shaking。webpack2开始，也加入了这个功能。

## 那么webpack的tree-shaking要如何开启呢？

其实很简单，需要有几个前提条件：

* webpack2.0及以上
* 使用了es6的模块语法导出导入
* 使用了uglifyJS压等压缩工具压缩代码

## 发现的问题

以上是网上说的比较多的，但是本人在使用过程中发现，还需要注意一个细节，那就是在配置babelrc（指所有配置babel的配置，其他可配置babel的地方如webpack配置babel-loader的options，还有package.json）时虽然可以归并到第一个前提条件，但是容易被忽略。

这个细节就是，配置presets是，一定要将modules设置为false，这个如果没设置，就没卵用，作者亲身体会。现在看看，这个设置理所当然，因为即使你使用了es6的语法来处理模块，在没设置这个参数时，默认是true，也就是默认将es6等的语法转换成common.js。

下面用实验代码来证实一下吧。（本次使用webpack3）

## 没有配置modules为false即modules默认值为true

index.js文件代码

```
import { a } from './module'

a()
```
module.js文件代码

```
export function a() {
  console.log('a');
}

export function b() {
  console.log('b');
}
```

webpack.config.js文件代码

```
const path = require('path')
const webpack = require('webpack')

module.exports = {
  entry: path.resolve(__dirname, 'index'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  module: {
    rules: [{
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['env']
        }
      }
    }]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin()
  ]
}
```

打包得到的文件index.bundle.js代码，注意，此时打包的文件里面是可以查到有关b函数的定义的，并没有shaking掉。

```
!function(n){function e(o){if(t[o])return t[o].exports;var r=t[o]={i:o,l:!1,exports:{}};return n[o].call(r.exports,r,r.exports,e),r.l=!0,r.exports}var t={};e.m=n,e.c=t,e.i=function(n){return n},e.d=function(n,t,o){e.o(n,t)||Object.defineProperty(n,t,{configurable:!1,enumerable:!0,get:o})},e.n=function(n){var t=n&&n.__esModule?function(){return n.default}:function(){return n};return e.d(t,"a",t),t},e.o=function(n,e){return Object.prototype.hasOwnProperty.call(n,e)},e.p="",e(e.s=1)}([function(n,e,t){"use strict";function o(){console.log("a")}function r(){console.log("b")}Object.defineProperty(e,"__esModule",{value:!0}),e.a=o,e.b=r},function(n,e,t){"use strict";(0,t(0).a)()}]);
```

## 配置modules为false

**对webpack.config.js的配置更改部分**

```
options: {
  presets: [
    ['env', {modules: false}]
  ]
}

```

**打包结果---无b函数定义**

```
!function(e){function t(r){if(n[r])return n[r].exports;var o=n[r]={i:r,l:!1,exports:{}};return e[r].call(o.exports,o,o.exports,t),o.l=!0,o.exports}var n={};t.m=e,t.c=n,t.i=function(e){return e},t.d=function(e,n,r){t.o(e,n)||Object.defineProperty(e,n,{configurable:!1,enumerable:!0,get:r})},t.n=function(e){var n=e&&e.__esModule?function(){return e.default}:function(){return e};return t.d(n,"a",n),n},t.o=function(e,t){return Object.prototype.hasOwnProperty.call(e,t)},t.p="",t(t.s=1)}([function(e,t,n){"use strict";function r(){console.log("a")}t.a=r},function(e,t,n){"use strict";Object.defineProperty(t,"__esModule",{value:!0});var r=n(0);n.i(r.a)()}]);
```

完！