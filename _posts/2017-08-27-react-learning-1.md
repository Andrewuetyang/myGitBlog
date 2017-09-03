---
layout: single
title: react框架入门小结
date: 2017-08-27 16:20 +0800
category: react
---
### 概要
前端框架纷繁复杂，多样的今天，似乎要学习的东西特别多。当然，不得不说，大家的时间都是有限的，而且大多数人是不能够做到大神般的领悟能力，即用极少的时间学到大量的知识。由于本人最先接触的两个框架并实际在工作中应用的是angular 1.x和vue，自然在学习react的时候，我会频繁拿他们来做为对比，毕竟对比学习，是人类常见的一种学习方式。所以在我看来，根据前面的应用框架经验，前端框架的学习主要集中在这几个方面。
* 核心理念
* 语法
* 数据绑定
* 列表渲染
* 条件渲染
* 事件处理
* 组件系统
* 路由系统

# react核心理念
![react](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1503855069490&di=3d14d9d197e81731150b0f2e726e6e87&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D3843805054%2C4208752828%26fm%3D214%26gp%3D0.jpg)

react在优化web技术性能方面，提出了非常具有革命性意义的方案。简单的说，有三个方面：

* 虚拟dom技术
* JSX语法
* 组件系统

这三个方面很好地解决了web性能、开发效能、代码维护性的问题。当然优化没有绝对完美，这个过程还将持续...

### 为什么采用JSX语法？
JSX语法，在很多人初次接触时，觉得很不友好，大量的HTML都往js里写，非常让人不适应，有一种原罪感。但是，你如果了解react的组件系统，你就知道，这样做的意义有多大。JSX语法其实是服务于组件编写的。react的组件化野心很大，react提供的组件玩法，如果玩家在拆分组件得当的情况下，后面的玩法就像撘积木一样，组合好就行。而组件的写法，可以用写类的方式来写，妥妥的契合了面向对象的思想，而这种思想，大量的存在于各种软件项目中，是程序员最为熟悉的一种编程思想。

### 数据绑定
jsx语法其实已经包括了数据如何绑定到你想要绑定的位置上，简单的说jsx语法使用`{}`花括号的方式，可以将`state, props`等数据绑定到你想要绑定的位置。`{}`里面可以是js表达式。如：

```
import React from 'react';
import ReactDom from 'react-dom';

let data = {
  id: 123,
  text: 'hello world'
};

ReactDom.render(
  <div>{data.text}</div>,
  document.getElementById('root')
);
```

### 列表渲染
相比于其他框架，如angular、vue，react没有提供指令系统，自然对于列表渲染的处理方式会有所不同。其实列表渲染的方式，主要还是由先决的jsx语法和组件系统决定的。一个简单的例子可以说明一下其列表渲染的方式(例子来自react官网)。这个方式首先给我留下一个影响，相比于angular和vue来说，不够简洁。虽然并不妨碍什么。

```
import React from 'react';
import ReactDom from 'react-dom';

const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);

ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);

```

### 条件渲染
对于需要条件渲染的部分，我们可以提取出来作为一个组件，这样，在组件里面，我们就可以对其做程序流的处理，如简单的渲染还是不渲染，就可以通过判断，返回还是不返回对应的`html`片段。当然如果只是渲染文本节点，也可以在`jsx`语法里面直接使用js表达式来做条件渲染。

### 事件处理
`react`事件处理其实还是组件系统中的一部分，其基本语法如下：

```
import React, {Component} from 'react';

class Test extends Component {
  constructor (props) {
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick(e) {
    // handle code go here
  }
  render() {
    return (
      <button onClick={handleClick}>点击</button>
    );
  }
}

```

### 组件系统
借助于`jsx`语法，`react`的组件可以很灵活，这也是它的特色之一。组件系统，解决了数据绑定、列表渲染、条件渲染和事件处理等问题。写高可维护性、高可扩展性的组件，是我们追求的一个事，也可以从某种程度上说明一个程序员的经验和能力。一般来说，编写组件需要考虑以下几个问题：

* 如何做到业务与组件逻辑分离？
* 组件的细粒度要做到什么程度？
* 组件的接口是否预留完备？

### 路由系统
当前越来越多的web应用采用`SPA`即单页面应用的技术，而单页面应用不得不提到的就是其路由系统。`react`中，官方也一直在维护其路由解决方案，即`react-router`。当前，该路由系统已经升级到4.x版本，可谓变化之快。路由的使用，可以参考[`react-router`官方文档](https://reacttraining.com/react-router/)，这里就不做介绍了。

(完)