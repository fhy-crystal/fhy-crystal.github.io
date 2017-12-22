---
layout: post
title: vue build 后部署无法加载css样式
excerpt: npm run build后部署dist文件夹，样式丢失
categories: [笔记, vue, mime]
tags: [笔记, vue, mime]
modified: 2017-12-22
comments: true
---

## 问题描述
像之前的项目一样build完后，将dist文件夹丢给运维，让其进行部署，但是这次出现了一个奇怪的问题，部署完后，运维反应页面怎么这么简陋。听到后当时还在想虽然功能简单，页面怎么也不至于用简陋来形容吧。。。打开链接瞬间傻眼，css样式全部没有应用，只有原生的一个框在页面上显示着，真的是很简陋。。。

baidu、segmentfault、Stack Overflow 各种搜索，问题指向了浏览器加载css的type问题。查看项目果然就是这个问题！！！css文件的Content-Type从`text/css`变成了`text/plain`

![css_mime-type](http://oy41mkgad.bkt.clouddn.com/css_mime_type.png 'css_mime-type')

## 解决方案

罪魁祸首是服务器忘记加载`mime.types`的文件，导致所有的文件都被强制转换成`text/plain`，加载了之后问题就解决啦！！！撒花❀~~