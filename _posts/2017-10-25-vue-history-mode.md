---
layout: post
title: VUE history 模式
excerpt: vue router 路由中 history 模式，去除项目路径中的#
categories: [教程, vue]
tags: [教程, vue]
modified: 2017-10-25
comments: true
---

vue-router 默认 hash 模式，使用 URL 的 hash 来模拟一个完整的 URL，好处是当 URL 改变时，页面不会重新加载。

但是之前碰到一个项目，需要解析路径，然后项目路径自带的 `#` 非常影响体验。下面就让我来唠叨唠叨如何去掉这个 `#` 吧

## 修改 VueRouter 
{% highlight js %}
export detault new Router({
	mode: 'history',
	routers: [{
		path: '/login',
		name: 'Login',
		component: Login
	},...]
})
{% endhighlight %}
当配置了`mode: 'history'`，URL 就由原先的`http://yoursite.com/#/login`变为`http://yousite.com/login`了。

如果直接由链接跳转，页面完全没有问题，但是如果你在跳转后的页面刷新，或者直接访问页面，就会返回404。所以，如果配置了`mode: 'history'`，就需要后台配置支持。

## 后台配置
我用的是PHPstudy的程序集成包，下载地址在[这里](https://www.baidu.com/link?url=yRcr1XyRiVvvVrxIGwUYj2-EAH2Bq0uXQvDpAeJpK8BTMCWyHyIC0pm5gAy6lHOWC4jASLGaOwTtcZa78XITbc9iC4tSLXaJQufyzZ_Bw37&wd=&eqid=beac4e620000592f0000000559edbd24)，安装完PHPstudy后，默认采用的是Apache和MySql模式的，虽然Vue的官网有提到apache如何配置，但是我配置完后，进入内部页面，刷新还是会出现404，所以这里只说Nginx的配置。

1. 首先把PHPstudy的模式由Apache切换为Nginx，点击下图圈出位置选择模式，我的是`php-5.2.17 + Nginx`

    ![phpstudy](http://oy41mkgad.bkt.clouddn.com/phpStudy.png "phpstudy")

2. 修改nginx.conf文件，点击 `其他选项菜单 > 打开配置文件 > nginx-conf`

    ![nginx](http://oy41mkgad.bkt.clouddn.com/nginx.png "nginx")

    内容修改如下：

    ![Origin](http://oy41mkgad.bkt.clouddn.com/origin.png "Origin")
    ![New](http://oy41mkgad.bkt.clouddn.com/new.png "New")

3. 重启PHPstudy，404的问题就解决啦~撒花❀~

## 优化
相信很多小伙伴在www底下会放着很多项目，之前的配置直接修改了80端口的设置，会导致其他项目都无法被正常打开了，所以在此进行优化。
同样打开`nginx-conf`文件，保留原先的80端口默认的设置，并在下面复制一份，然后再修改。如下图

![nginxFix](http://oy41mkgad.bkt.clouddn.com/nginx_fixed.png "nginxFix")

这样要访问该项目直接在浏览器输入`localhost:9080`即可，这样就不会影响其他项目，该怎么打开就怎么打开。