---
layout: post
title: VUE WEBPACK 可配置文件
excerpt: vue webpack 打包后，可以直接通过配置文件修改内容，无需重新打包
categories: [教程, vue]
tags: [教程, vue]
comments: true
---

通常一个项目的开发，都会部署到三个环境中。

`开发环境：`供开发自己调试接口，解决bug。

`测试环境：`供测试人员测试，提出bug。

`线上环境：`供用户使用。

这三个环境中的数据应该是相互独立，互不影响的，所以就会存在3个后台接口地址。那不能每次要发布到不同的环境上，都得手动修改在打包，这样太麻烦啦！

最好的办法就是将url单独提取到一个文件中，每次要修改路径了，直接改这个文件就能达到目的。

然后我们就开始提取工作~

## 创建文件
`static > js` 在这个路径下创建`serverurl.js`，内容为后台接口的ip地址（一般三个环境接口的区别就是ip不同）
{% highlight js %}
export default 'http://xx.xx.xx/'
{% endhighlight %}

为什么放在`static`里面，而不是`src > assets`里呐？通过实践证明：运行`npm run build` 后生成的`dist`文件夹中，不会存在`assets`文件夹里面的文件，他们全部被打包到一起啦！而只要是放在`static`文件夹里面的，都幸免于难。因为我们最后是要修改`serverurl.js`这个文件的，所以不能让它被打包了！

## 引用文件
哪里需要用到后台接口的ip地址，就用以下代码
{% highlight js %}
import serverurl from '../static/js/serverurl' //'../static/js/serverurl'是具体项目中serverurl的位置，这里只做演示
{% endhighlight %}

然后就进行正常的开发流程，最后`npm run build`，修改`dist > static > js > serverurl.js`里面的ip地址，然后将文件夹里面的内容部署到服务器。大功告成！

### 出现问题
但是！！！你会发现刚刚明明已经修改了`serverurl.js`的地址，但是接口访问的还是修改前的，到底哪里出了问题！！？？

通过搜索，发现修改前的地址被打包进了app.xxx.js.map和app.xxx.js，虽然我们修改了地址，但是看来打包后应用的并不是我们的`serverurl.js`:(

## 改造
感谢[请叫我流川枫好吗](http://www.cnblogs.com/caimuqing/p/7094364.html)提供思路，新方案不采用import方式引入ip

### 修改`serverurl.js`内容
{% highlight js %}
"http://xx.xx.xx/"
{% endhighlight %}
经实践标明，一定要用`双引号`！除了ip地址什么都不要填！理由下面会说明

### 修改引入方式
{% highlight js %}
axios.get('static/js/serverurl.js').then((data) => {
    localStorage.setItem('serverurl', response.data);//由于是异步操作，为了通信，可以采用localstorage，把东西存起来
}, (data) => {
    console.log(data)
})
{% endhighlight %}

可以看到这里采用的是直接读取的方式，`serverurl.js`里面的所有内容会作为response.data存储到localStorage里面，所以如果使用的是单引号，那么读到的数据会是`'http://xx.xx.xx/'`这种形式，如果写入注释等一样会被全部读取到

### 获取ip地址
哪里需要ip地址，直接从`localStorage`里面读取即可
{% highlight js %}
localStorage.getItem('serverurl')
{% endhighlight %}

这样再进行打包发布的时候，直接修改`serverurl.js`ip地址，修改完刷新就能立马生效！

大功告成~撒花❀~~
