---
title: React第六讲课堂笔记（事件）
date: 2016-05-12 13:22:01
tags: React
categories: React
---
# React第五讲课堂笔记（DOM操作）

## 1. `React 中获取DOM的两种方式`
- ReactDOM.findDOMNode
- this.refs.xxx

获取DOM后可以方便结合现有非 react 类库的使用，通过 ref/refs 可以取得组件实例，进而取得原生节点，不过尽量通过 state/props 更新组件，不要使用该功能去更新组件的
# React第六讲课堂笔记（事件）

可以通过设置原生 dom 组件的 onEventType 属性来监听 dom 事件，例如 onClick, onMouseDown，在加强组件内聚性的同时，避免了传统 html 的全局变量污染

```
'use strict';

import React, { Component } from 'react';

class HandleEvent extends Component {

  state = { liked: false }

  handleClick = (event) => {
    this.setState({liked: !this.state.liked});
  }

  render() {
    let text = this.state.liked ? '喜欢' : '不喜欢';

    return (
      <p onClick={this.handleClick}>
        我 {text} 你.
      </p>
    );
  }
}

export default HandleEvent;

```

**注意：事件回调函数参数为标准化的事件对象，可以不用考虑 IE**

更多事件我们可以一起看[这里](http://reactjs.cn/react/docs/events.html#form-events)
