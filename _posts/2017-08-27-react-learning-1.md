---
layout: single
title: react框架入门小结
date: 2017-08-27 16:20 +0800
category: react
---
### 概要
前端框架纷繁复杂，多样的今天，似乎要学习的东西特别多。当然，不得不说，大家的时间都是有限的，而且大多数人是不能够做到大神般的领悟能力，即用极少的时间学到大量的知识。在我看来，前端框架的学习主要集中在这几个方面。
* 核心理念
* 语法
* 数据绑定
* 列表渲染
* 条件渲染
* 事件处理

# react核心理念
![react](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1503855069490&di=3d14d9d197e81731150b0f2e726e6e87&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D3843805054%2C4208752828%26fm%3D214%26gp%3D0.jpg)

react在优化web技术性能方面，提出了非常具有革命性意义的方案。简单的说，有三个方面：

* 虚拟dom技术
* JSX语法
* 组件系统

这三个方面很好地解决了web性能、开发效能、代码维护性的问题。当然优化没有绝对完美，这个过程还将持续...

### 为什么采用JSX语法？
JSX语法，在很多人初次接触时，觉得很不友好，大量的HTML都往js里写，非常让人不适应，有一种原罪感。但是，你如果了解react的组件系统，你就知道，这样做的意义有多大。JSX语法其实是服务于组件编写的。react的组件化野心很大，react提供的组件玩法，如果玩家在拆分组件得当的情况下，后面的玩法就像撘积木一样，组合好就行。而组件的写法，可以用写类的方式来写，妥妥的契合了面向对象的思想，而这种思想，大量的存在于各种软件项目中，是程序员最为熟悉的一种编程思想。
