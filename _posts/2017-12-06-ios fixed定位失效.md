---
layout: post
title: ios fixed定位失效
date: 2017-12-06
categories: blog
tags: [resource]
description: 解决ios中fixed定位失效的方法
---

ios中fixed定位的元素在弹出软件盘的时候会出现错位。查阅资料后，想到了一个解决方法---使用absolute模拟fixed，该方法已在ios上验证可行。

```css
  *{
    margin: 0;
    padding: 0;
  }
  html, body {
    -webkit-overflow-scrolling: auto;
    /* 不能 postion: relative; */
  }
  header, footer, article {
    position: absolute;
    left: 0;
    width: 100%;
    line-height: 50px;
    background-color: #fff;
  }
  header, footer {
    z-index: 2;
    height: 50px;
    text-align:center;
  }
  header {
    top: 0;
    border-bottom: 1px solid #ccc;
  }
  footer {
    bottom: 0;
    border-top: 1px solid #ccc;
  }
  article{
    z-index: 1;
    top: 0;
    height: 100%; /* 必须要约束height*/
    box-sizing: border-box;
    padding: 51px 0;
    overflow-y: auto;
    overflow-x: hidden;
    -webkit-overflow-scrolling: touch;
    background-color: #f2f2f2;
  }
```

```html
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-touch-fullscreen" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="format-detection" content="telephone=no">
  </head>
  <body>
    <header>header</header>
      <article>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
        <p>内容超出高度时，灰色区域可上下滑动</p>
      </article>
    <footer>footer</footer>
  </body>
</html>
```