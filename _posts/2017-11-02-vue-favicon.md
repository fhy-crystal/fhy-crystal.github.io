---
layout: post
title: VUE 设置 favicon
excerpt: 设置faviocn
categories: [教程, vue]
modified: 2017-11-02
comments: true
---

## 问题描述
使用 webpack + vue-cli 构建的项目，打包发布后，用浏览器打开，我们通常看到标签页是长这样的

![tagNull](http://oy41mkgad.bkt.clouddn.com/tagNull.png "tagNull")

## 解决方案

1. 在根目录放置`favicon.ico`图片文件，生成方式在[这里](http://tool.lu/favicon/)

2. 在`build/webpack.prod.conf.js`中的`HtmlWebpackPlugin`传入参数`favicon: 'favicon.ico'`，如图：

    ![faviconConfig](http://oy41mkgad.bkt.clouddn.com/faviconConfig.png "faviconConfig")

3. 打包发布后，我们能看到标签页变成了这样。

    ![tag](http://oy41mkgad.bkt.clouddn.com/tag.png "tag")