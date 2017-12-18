---
layout: page
title: "分类"
description: '文章分类'  
header-img: "img/semantic.jpg"  
---

## 本页使用方法

- 在下面选一个你喜欢的词
- 点击它
- 相关的文章会「唰」地一声跳到页面顶端
- 马上试试？

## 关键字列表


<div id='tag_cloud'>
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a>
{% endfor %}
</div>


{% for tag in site.tags %}
<h4 class="listing-seperator" id="{{ tag[0] }}">{{ tag[0] }}</h4>
<ol class="listing">
{% for post in tag[1] %}
  <li class="listing-item">
  <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ol>
{% endfor %}

<script src="/media/js/jquery.tagcloud.js" type="text/javascript" charset="utf-8"></script> 
<script language="javascript">
$.fn.tagcloud.defaults = {
    size: {start: 1, end: 1, unit: 'em'},
      color: {start: '#f8e0e6', end: '#ff3333'}
};

$(function () {
    $('#tag_cloud a').tagcloud();
});
</script>
