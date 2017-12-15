---
layout: post
title: 利用sessionStorage保存列表页数据
date: 2017-12-15
categories: blog
tags: [javascript]
description: 利用sessionStorage保存列表页数据及sessionStorage特性记录
---

在之前的学习工作中因为应用场景的原因对于*sessionStorage*接触的并不多，大部分时间都没有考虑过*sessionStorage*。之前需要存储数据的时候，少量数据存*cookie*，大量数据存*localStorage*。
前几天在做公众号钓场列表页的时候，突然想到可以用*sessionStorage*缓存数据，这样用户在从详情页返回列表页的时候，列表页可以还原成之前的样子，也不用重复请求数据了。使用*sessionStorage*的一大好处就是，数据在回话关闭后自动清除，这样进入列表页不会被以前缓存的数据干扰。

下面是使用过程：
- 点击跳转详情页用click事件而不是a标签，以便我们知道何时需要缓存数据；
- 点击时间中将可能用到的数据写入一个object，以字符串的形式存入sessionStorage；
- 页面返回时在sessionStorage查找时候存在缓存数据，是则还原数据，否则将缓存数据写入页面。

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
需要注意的是 *sessionStotage*、*localStorage*、*cookie*都只能存储字符串。
*sessionStotage* 浏览器（当前标签）关闭，或者手动改变网址的时候会清除数据，浏览器多个同域标签无法互相访问*sessionStotage*。