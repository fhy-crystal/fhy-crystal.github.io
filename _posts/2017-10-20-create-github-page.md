---
layout: post
title: 创建自定义 Github 页面
excerpt: 用 GitHub + Jekyll 创建自定义的 GitHub 页面
categories: [教程]
tags: [教程]
modified: 2017-10-20
comments: true
---

此博客的由来很简单，想要一个显得高大上的个人页面:)


下面开始搭建流程。

## 准备Github的账号

在`github`网站注册一个账号

## 新建username.github.io项目
1. 在 github 主页右下角找到 `New repository` 按钮

    ![New repository](http://oy41mkgad.bkt.clouddn.com/githubNewRepository.png "New repository")



2. 开始创建新的仓库，仓库名称为`用户名.github.io`，比如我的用户名是fhy-crystal，所以新建的仓库名为`fhy-crystal.github.io`，其他都不需要修改，直接点击下面的`Create repository`按钮

    ![Creare repository](http://oy41mkgad.bkt.clouddn.com/githubCreateRepository.png "Create repository")


3. 克隆项目到本地，我用的是GitHub客户端，创建一个 index.html 文件，随便写点什么内容，提交到 github 上
    
    ![Clone repository](http://oy41mkgad.bkt.clouddn.com/cloneRepository.png "Clone repository")


4. 访问`https://用户名.github.io/`，就能看到刚刚index文件里面输入的内容啦。到此，工程已完成一半~

## 安装Jekyll

1. 安装`ruby`，下载地址在[这里](https://rubyinstaller.org/downloads/)，然后正常的一直下一步下一步，直到提示完成。在cmd中输入

```
    ruby -v
```
 如果显示版本号则表示安装成功

2. 安装`jekyll`，在cmd中输入

```
    gem install jekyll
```
安装完成后运行

```
    jekyll -v
```
如果显示版本号则表示安装成功

3. 新建jekyll项目，并启动项目

```
    jekyll new testProject
    cd testProject
    jekyll serve
```
如果没有报错，在http://localhost:4000就能看到默认的页面啦，这样就进展到80%啦~

4. 但是，我在配置jekyll的时候出现的各种错误，提示我各种文件缺失！但是不要慌，再cmd中输入

```
    gem install 提示缺失的文件
```
安装完后在运行

```
    jekyll serve
```
如果还报错，那就再安装，再运行，直到没有缺失文件为止，就会出现步骤3提到的默认页面啦，然后jekyll的配置已经完成啦，这个项目你可以保留，也可以删除，这就是为了配置jekyll才被创建的

## 寻找Jekyll模板
Jekyll有好多好多模板，找一个看着顺眼的很重要，不然写博客的时候没有好心情那就糟糕啦~推荐地址在[这里](http://jekyllthemes.org/)，你可以找一个然后download下来

## 定制github.io
1. 将下载过来的Jekyll模板copy到从本地的github.io项目中

2. 在_config.yml中将信息修改为你个人的信息

3. 然后在cmd中运行

```
    jekyll serve
```
查看修改效果

4. 如果觉得Ok啦，就可以提交到github上

然后访问`https://用户名.github.io/`，就会发现网站已经不是只有一个index.html的寒酸样啦

## 编写博客
框架已经搭好，可以动手写博客啦。将文件放在_posts文件夹中，文件名必须为“年-月-日-文章标题.后缀名”，比如2017-10-20-create-github-page.md。如果网页代码采用html格式，后缀名为html；如果采用markdown格式，后缀名为md。


内容编写，头部必须包含以下内容

```
---
layout: post // 文章的模板
title: 创建自定义 Github 页面 // 文章标题
excerpt: 用 GitHub + Jekyll 创建自定义的 GitHub 页面 // 摘要
categories: [教程] // 归类
modified: 2017-10-20 // 修改日期
comments: true
---
```
博客中免不了需要插入图片，这时候就该图床上场啦，我用的是七牛图床，10G的免费空间，应该能用挺久的吧~使用方法的话[百度一下](http://www.baidu.com)你就知道:)



然后重复 `定制github.io`中的3和4步骤