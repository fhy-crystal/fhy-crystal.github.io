---
layout: post
title: vue中使用better-scroll
excerpt: 在vue中使用better-scroll遇到的一些问题总结
categories: [笔记, vue, better-scroll]
tags: [笔记, vue, better-scroll]
modified: 2018-01-04
comments: true
---

## 1. 初始化
在vue中初始化better-scroll
{% highlight html %}
<template>
    <div class="wrapper" ref="wrapper">
        <ul class="content" ref="content">
            <li>...</li>
            <li>...</li>
            ...
        </ul>
    </div>
</template>
<script>
    import BScroll from 'better-scroll'
    export default {
        mounted() {
            this.$nextTick(() => {
                let options = {};
                this.scroll = new Bscroll(this.$refs.wrapper, options)
            })
        }
    }
</script>
{% endhighlight %}

vue 提供了一个获取DOM对象的接口 `vm.$refs` 。在这里，我们通过了`this.$refs.wrapper`访问到了这个 DOM 对象，并且我们在 mounted 这个钩子函数里 `this.$nextTick` 的回调函数中（确保 DOM 已经加载）初始化 better-scroll 。因为这个时候，wrapper 的 DOM 已经渲染了，我们可以正确计算它以及它内层 content 的高度，以确保滚动正常

我们还可以在 options 中配置一些参数，具体内容请参照[选项/基础](https://ustbhuangyi.github.io/better-scroll/doc/zh-hans/options.html)和[选项/高级](https://ustbhuangyi.github.io/better-scroll/doc/zh-hans/options-advanced.html)


## 2. 初始化后无法滚动
滚动的原理相信大家都有所了解，只有当内容超过容器时，才会出现滚动条，以下是better-scroll 常见的html结构，
{% highlight html %}
<div class="wrapper" ref="wrapper">
    <ul class="content" ref="content">
        <li>...</li> 
        <li>...</li> 
        ... 
    </ul> 
</div>
{% endhighlight %}

![scroll](http://oy41mkgad.bkt.clouddn.com/scroll-4.png 'scroll')

所以想要让 better-scroll 的内容滚动起来， `content` 的高度就一定得大于 `wrapper` 的高度。

在构建元素的时候，我们可以给 `wrapper` 设置一个固定的高度， `content` 的高度则随内容增多而增加。

但是我们会碰到一种极端的情况，当 `content` 里面的内容比较少的时候，无法撑起它的高度，这个时候，我们就需要手动来设置 `content` 的高度，让他能够超过 `wrapper` ，具体做法如下：

{% highlight js %}
<script>
    export default {
        mounted() {
            this.$nextTick(()=>{
                this.initScroll();
            })
        },
        methods: {
            initScroll() {
                if (this.$refs.content) {
                    if (this.$refs.wrapper instanceof window.SVGElement) {
                        let rect = this.$refs.wrapper.getBoundingClientRect();
                        this.$refs.content.style.minHeight = `${rect.height + 1}px`
                    } else {
                        this.$refs.content.style.minHeight = `${this.$refs.wrapper.offsetHeight + 1}px`
                    }
                }
            }
        }
    }
</script>
{% endhighlight %}

取得 `wrapper`的高度，`+1px`后赋给 `content`，这样就无须担心内容太少而出现无法滚动的情况啦

## 3. 实现上滑加载和下拉刷新
{% highlight html %}
<template>
    <div class="wrapper" ref="wrapper">
        <ul class="content" :style="pullDownStyle" ref="content">
            <li v-for="item in data">{{item}}</li>
        </ul>
        <div class="loading-wrapper top" v-show="pullingDown"></div>
        <div class="loading-wrapper" v-show="isPullingUp"></div>
    </div>
</template>
<script>
    import BScroll from 'better-scroll'
    export default {
        data() {
            return {
                data: [],
                isPullingUp: false,
                isPullingDown: false,
                beforePullDown: false,
                pullDownStyle: ''
            }
        },
        created() {
            this.loadData()
        },
        methods: {
            loadData() {
                requestData().then((res) => {
                    this.data = res.data.concat(this.data)
                    this.$nextTick(() => {
                        if (!this.scroll) {
                            let options = {
                                pullDownRefresh: {
                                    threshold: 50, // 当下拉到超过顶部 50px 时，触发 pullingDown 事件
                                    stop: 0 // 刷新数据的过程中，回弹停留在距离顶部还有 0px 的位置
                                },
                                pullUpLoad: {
                                    threshold: -20, // 在上拉到超过底部 20px 时，触发 pullingUp 事件
                                },
                                click: true //better-scroll 默认会阻止浏览器的原生 click 事件。当设置为 true，better-scroll 会派发一个 click 事件
                            };
                            this.scroll = new Bscroll(this.$refs.wrapper, options);
                            // 下拉刷新
                            this.scroll.on('pullingDown',() =>{
                                this.isPullingDown = true;
                                this.beforePullDown = true;
                                this.loadData();
                            })
                            // 上拉加载
                            this.scroll.on('pullingUp',() =>{
                                this.isPullingUp = true;
                                this.loadData();
                            })
                            // 下拉刷新时显示loading
                            this.scroll.on('scroll', (pos) => {
                                if (this.beforePullDown) {
                                    this.pullDownStyle = `padding-top:3rem`;
                                }
                            })
                        } else {
                            if (this.isPullingUp) {
                                this.isPullingUp = false;
                                this.scroll.finishPullUp();
                            } else if (this.isPullingDown) {
                                // 取消loading显示
                                this.pullDownStyle = `padding-top:0`;
                                this.beforePullDown = false;
                                
                                this.isPullingDown = false;
                                this.scroll.finishPullDown();
                            }
                            this.scroll.refresh()
                        }
                    })
                })
            }
        }
    }
</script>
{% endhighlight %}

这里的 `requestData` 是伪代码，作用就是发起一个 http 请求从服务端获取数据，并且这个函数返回的是一个 promise（实际项目中我们可能会用 axios 或者 vue-resource）。我们获取到数据的后，需要通过异步的方式再去初始化 better-scroll，因为 Vue 是数据驱动的， Vue 数据发生变化（this.data = res.data）到页面重新渲染是一个异步的过程，我们的初始化时机是要在 DOM 重新渲染后，所以这里用到了 `this.$nextTick`。

为什么这里在 created 这个钩子函数里请求数据而不是放到 mounted 的钩子函数里？因为 requestData 是发送一个网络请求，这是一个异步过程，当拿到响应数据的时候，Vue 的 DOM 早就已经渲染好了，但是数据改变 —> DOM 重新渲染仍然是一个异步过程，所以即使在我们拿到数据后，也要异步初始化 better-scroll。

初始化 better-scroll 时，我们通过配置 options 打开`pullDownRefresh（下拉刷新功能）`和`pullUpLoad（上拉加载功能）`，具体可见[这里](https://ustbhuangyi.github.io/better-scroll/doc/zh-hans/options-advanced.html#pulldownrefresh)

然后通过监听`pullingDown`和`pullingUp`来实现数据的加载，加载结束后，我们需要通过调用`finishPullDown()`和`finishPullUp()`告诉 better-scroll 数据已加载

以上就是第一次使用 better-scroll 的总结，完结撒花❀~