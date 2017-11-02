---
layout: post
title: VUE 引入外部资源
excerpt: 解决 vue build 之后，项目中引入的图片、文字等路径出错问题
categories: [教程, vue]
modified: 2017-11-01
comments: true
---

## 问题描述

默认使用 webpack + vue-cli 构建的项目，打包的css和js等资源的路径都是绝对的。当部署到带有文件夹的项目中时，引入的资源都会404。原因在这里

![vueConfig](http://oy41mkgad.bkt.clouddn.com/vueConfig.png "vueConfig")

因为配置将`static`文件夹作为了根目录。

## 解决方案
将上图圈出的`assetsPublicPath: '/'`改为`assetsPublicPath: './'`，这样引入的资源就是相对路径啦~


## 又出现问题
css引入的问题是解决了，但是css中免不了会引入背景图或者文字，不知道小伙伴们有没有碰到过类似的问题，通过
{% highlight css %}
background-image: url('../assets/wave.png')
{% endhighlight %}
以上方式引入的背景图片，在 `npm run dev` 本地调试的时候能够完美运行，但是一旦`npm run build` 运行后，通过访问`dist>index.html`，就会出现未找到`wave.png`。文字也是一样。

## 解决方案
### 通过import的方法引入
{% highlight css %}
import waveImg from '../assets/wave.png';
{% endhighlight %}

{% highlight js %}
export default {
    data() {
        backgroundImg: {
            wave: waveImg
        }
    }
}
{% endhighlight %}

{% highlight html %}
<div :style="{background: 'url(' + backgroundImg.wave + ') no-repeat'}"></div>
{% endhighlight %}

但是这种方式太麻烦啦，这里只用到了一个资源，但是如果需要引入更多资源，那就得每个都import，然后export，再引用。ORZ，我选择狗带。。。

### 修改配置文件（墙裂推荐！）

在`build/utils.js`中的`ExtractTextPlugin.extract`传入参数`publicPath: '../../'`，完美解决背景图及字体引入问题！撒花❀~

![vueUtils](http://oy41mkgad.bkt.clouddn.com/vueUtils.png "vueUtils")
