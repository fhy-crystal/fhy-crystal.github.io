---
layout: post
title: 移动端1px
excerpt: 移动端实现1px像素线
categories: [教程]
modified: 2017-11-03
comments: true
---

## 问题描述
很多时候我们在css中写了1px，但是放到手机上看后，往往觉得会变粗很多，这都是2倍屏（dpr=2）、3倍屏（dpr=3）在捣鬼！所以导致css中的1px不等于移动设备的1px。

在window对象中有一个devicePixelRatio属性，他可以反应css中的像素与设备的像素比。devicePixelRatio的官方的定义为：设备物理像素和设备独立像素的比例，也就是 devicePixelRatio = 物理像素 / 独立像素。

常见的iphone4/iphone5/iphone6的devicePixelRatio=2，iphone6 plus的为3。具体的数据大家可以在chrome的console面板输入window.devicePixelRatio查看

![consoleBlock](http://oy41mkgad.bkt.clouddn.com/consoleBlock.png 'consoleBlock')


## 解决方案

### 1.1/window.devicePixelRatio 
{% highlight css %}
div {
    border: 1px solid #000
}
@media (-webkit-min-device-pixel-ratio: 2) {
    div {
        border: 0.5px solid red
    }
}
@media (-webkit-min-device-pixel-ratio: 3) {
    div {
        border: 0.3333px solid blue
    }
}
{% endhighlight %}
so easy!妈妈再也不用担心1px的问题~

但是！！！安卓和iOS8以下设备无法识别。so sad。。。

### 2.使用渐变
{% highlight css %}
div {
    background:
       linear-gradient(#000, #000) top / 100% 1px no-repeat,
       linear-gradient(#000, #000) right / 1px 100% no-repeat,
       linear-gradient(#000, #000) bottom / 100% 1px no-repeat,
       linear-gradient(#000, #000) left / 1px 100% no-repeat
}
{% endhighlight %}
但是这个如果边框有圆角的话就没办法实现啦

### 3.box shadow 模拟
{% highlight css %}
div {
    box-shadow: inset 0px 0px 0px 1px #000; /*模拟下边框*/
}
{% endhighlight %}
但是box shadow 最佳效果是模拟一条边框，4条边框的话粗细会不一样哦

### 4.viewport + rem（推荐）
在`devicePixelRatio = 2`时，输出viewport：
{% highlight html %}
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">
{% endhighlight %}

在`devicePixelRatio = 3`时，输出viewport：
{% highlight html %}
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">
{% endhighlight %}

这种方式就能和之前那样愉快的设置1px啦~可以引用淘宝的flexible.js文件，参考[这里](https://github.com/amfe/article/issues/17)

### 5.伪类 + transform
{% highlight css %}
.scale-1px{
    position: relative;
    border: 0;
}
.scale-1px:after{
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    border: 1px solid #000;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    width: 200%;
    height: 200%;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    -webkit-transform-origin: left top;
    transform-origin: left top;
}
{% endhighlight %}

这个方式如果 scale-1px 所在的容器含有浮动，4条边框就会出现不一样粗的问题，具体原因不清楚。。。所以如果要用这个方法，就尽量不要用浮动啦~~~

差不多完结啦~撒花❀~
