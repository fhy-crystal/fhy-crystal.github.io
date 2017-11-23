---
layout: post
title: 用 css 实现左边定宽，右边自适应
excerpt: 用 css 实现左边为固定宽度，右边宽度自适应
categories: [笔记, css]
tags: [笔记, css]
modified: 2017-11-23
comments: true
---

## 前期准备
{% highlight html %}
<body>
    <div class="left"></div>
    <div class="right"></div>
</body>
{% endhighlight %}

## 方法一：float
{% highlight css %}
.left {
    float: left;
    width: 100px;
    height: 200px;
    background-color: red;
}
.right {
    margin-left: 100px;
    height: 200px;
    color: white;
    background-color: blue;
}
{% endhighlight %}

## 方法二：absolute
{% highlight css %}
body {
    position: relative;
}
.left {
    width: 100px;
    height: 200px;
    background-color: red;
}
.right {
    position: absolute;
    top: 0;
    left: 100px;
    width: calc(100% - 100px); /*calc中运算符符号的前后都需要空格*/
    height: 200px;
    color: white;
    background-color: blue;
}
{% endhighlight %}

## 方法三：flex
{% highlight css %}
body {
    display: flex;
}
.left {
    width: 100px;
    height: 200px;
    background-color: red;
}
.right {
    flex: 1; /*flex-grow:1; flex-shrink:1; flex-basis:0%*/
    color: #fff;
    background-color: blue;
}
{% endhighlight %}