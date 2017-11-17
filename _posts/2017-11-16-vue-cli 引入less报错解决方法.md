---
layout: post
title: vue-cli 引入less报错解决方法
date: 2017-11-16
categories: blog
tags: [vue]
description: vue-cli 引入less报错解决方法
---

#### 之前用vue-cli搭建单页面开发环境时vue组件中引入less直接报错，后来网上搜索后找到解决方法。只需要两步：

1、安装 less-loader

```javascript
  npm install less-loader --save-dev
```

2、引入 less文件
重点在 lang='less' 和 src上

```javascript
  <style lang="less" src='less_url/notify.less'></style>
```