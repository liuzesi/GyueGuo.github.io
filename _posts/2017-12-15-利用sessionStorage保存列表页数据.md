---
layout: post
title: 利用sessionStorage保存列表页数据
date: 2017-12-15
categories: blog
tags: [javascript]
description: 利用sessionStorage保存列表页数据及sessionStorage特性记录
---

在之前的学习工作中因为应用场景的原因对于`sessionStorage`接触的并不多，大部分时间都没有考虑过`sessionStorage`，需要存储数据的时候，少量数据存`cookie`，大量数据存`localStorage`。

然而有这么一种场景：页面数据通过ajax异步加载，在跳转后返回时需要新获取数据，浏览状态无法保存，这个时候需要我们自己对数据进行缓存，之前实现这种功能用的`localStorage`，前两天突然想到`sessionStorage`，然后试了下发现确实这种情况使用sessionStorage更合适。

下面是使用过程：
- 点击跳转详情页用click事件而不是a标签，以便我们知道何时需要缓存数据；
- 点击时间中将可能用到的数据写入一个object，以字符串的形式存入`sessionStorage`；
- 页面返回时在`sessionStorage`查找时候存在缓存数据，是则还原数据，否则将缓存数据写入页面。

点击事件：

```javascript
function click {
  var data = {
    scrollTop: document.body.scrollTop || document.documentElement.scrollTop,
    entities: [] // 如果是列表页的话数据会是一个数组
  };
  sessionStorage.setItem('xxx', JSON.stringify(data)); // 将json转成字符串写入sessionStorage
  location.href = 'xxxx';
}

```

加载时还原数据

```javascript
var list = sessionStorage.getItem('xxx');
if (list) {  // 存在缓存
  list = JSON.parse(list); // 字符串转换成json
} else { // 不存在缓存
  ajax ...
}

```
需要注意的是
- `sessionStotage`、`localStorage`、`cookie`都只能存储字符串。
- `sessionStotage` 浏览器（当前标签）关闭，数据会被清除。
- 浏览器手动打开多个同域标签无法互相获取`sessionStotage`。
- 当前页面通过window.open 或者 a链接打开的同域子页面可以访问父页面的`sessionStotage`，子页面存在关闭父页面数据不会清除。
- 当前页面跳转另一域页面无法获取数据，返回页面可以获取。