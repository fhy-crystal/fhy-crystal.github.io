---
layout: post
title: html5新增download属性
excerpt: html5 a标签中新增的download属性测试
categories: [笔记, html5]
tags: [笔记, html5]
modified: 2017-11-24
comments: true
---

## 页面实现下载功能
一般我们在前端实现下载功能，都是采用a标签的形式
{% highlight html %}
<a href="http://oy41mkgad.bkt.clouddn.com/downloadtest.rar">文件下载</a>
{% endhighlight %}
这是rar的文件就能被下载到啦，但是如果我们需要下载的是图片，采用上述方式，浏览器就会直接打开图片，而不是下载，因为浏览器可以直接浏览图片。

## 解决问题
在download属性出来之前，我一直没办法处理这个问题，只能将图片展示在页面上，然后右键`图片另存为`将其保存下来

现在！！哈哈哈，终于不用这么用这么鸡肋的方法啦！！！

{% highlight html %}
<a href="http://oy41mkgad.bkt.clouddn.com/createdDB.png" download="createdDB">文件下载</a>
{% endhighlight %}

通过`download`属性还能进行重命名的操作~~

## 兼容性
![caniuse_download](http://oy41mkgad.bkt.clouddn.com/caniuse_download.png 'caniuse_download')