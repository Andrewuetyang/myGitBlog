---
layout: single
title:  微信小程序编写经验分享
date:   2017-08-04 21:04:08 +0800
categories: front-end
---

1. 微信开发者工具：小程序的开发，需要使用微信开发者工具，小程序需要编译才能执行。注意不要文件保存时自动编译。编译是一个比较耗时的过程。这样非常影响开发效率和体验，还是手动编译好；

2. 数据绑定：小程序是没有数据双向绑定的概念，是单向绑定。要实现双向绑定，可以利用事件；

3. 阻止事件冒泡。小程序中有冒泡事件和非冒泡事件的概念。bind开头的事件是冒泡事件，catch开头的事件是非冒泡事件；

4. 事件绑定：a.事件绑定如何往事件处理函数中传值？小程序在事件处理中会传入一个event对象，可通过event对象找到target(或currentTarget)下面的dataset。dataset对应标签上属性data-*里指向的值;b.event对象下的target和currentTarget的区别。target简单的说是触发事件的源组件；currentTarge是指事件绑定的当前组件。

5. 跳转：navigateTo跳转是跳转到一个新页面，页面栈的情况是push一个页面入栈，但对上一个页面不做处理。redirectTo跳转到一个新页面，页面栈的情况是push一个页面入栈，但会干掉上一个页面。navigateBack跳转是从当前这个页面在页面栈的位置中回退到上一个页面。reLaunch跳转是把所有页面栈清掉，在push一个新页面入栈。

6. 组件化（小程序中叫模板）：关键字是template标签和引入标签import。

7. 可以使用parseInt等api吗？ 可以使用setTimeout等api吗？ 都可以

8. 如何获取url中的参数？onLoad函数中自带有个options参数，这个options就是处理好的url参数对象。

9. es6支持度，暂时不支持for of的object.entries方法。

10. 对于富文本的解析，可以使用wxParse这个开源工具。其原理是将富文本转化为一个json数据格式，再通过模板template进行渲染。

11. 改变checkbox的大小，需要使用transform来做缩放。