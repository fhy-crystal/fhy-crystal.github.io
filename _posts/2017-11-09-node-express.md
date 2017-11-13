---
layout: post
title: 使用 node + express 搭建简单的后台
excerpt: 使用 node + express 搭建简单的后台
categories: [教程, node, express]
tags: [教程, node, express]
modified: 2017-11-09
comments: true
---

## 目录结构
nodeExpress

    ├─server.js

    ├─index.html

    ├─node_modules

## 前期准备
在nodeExpress文件夹中安装express和body-parser
{% highlight js %}
npm install express body-parser --save-dev
{% endhighlight %}

## 文件解析
### server.js
{% highlight js %}
var express = require('express'); // 引入express
var bodyParser = require('body-parser'); // 引入body-parser

var app = express(); // 创建expresss

//json编码
app.use(bodyParser.json()); 

//设置允许跨域访问
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With,Content-Type");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

// request 对象表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性
// response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据
app.get('/formsubmit', function(req, res) {
    // req.query 获取表单形式传递的参数（URL的查询参数串）
    var response = {
       firstName: req.query.first_name,
       lastName: req.query.last_name
    };
    console.log(response);
    res.send(JSON.stringify(response));
})

app.post('/postTest', function(req, res){
    // req.body 获取json格式传递的参数
    console.log(req.body);
    var resBody = {
        status: 0
    };
    res.json(resBody);
    // res.send(JSON.stringify(resBody)); // 返回数据
})

//监听8081接口打印请求域名和端口
var server = app.listen(8081, function () {

    var host = server.address().address;
    var port = server.address().port;

    console.log("应用实例，访问地址为 http://%s:%s", host, port);

})
{% endhighlight %}

### index.js
{% highlight html %}
    <section>
        <h2>FORM表单格式</h2>
        <form action="http://localhost:8081/formsubmit" method="GET" enctype="text/plain">
            First Name: <input type="text" name="first_name">
            <br>
            Last Name: <input type="text" name="last_name">
            <br>
            Submit: <input type="submit" value="Submit">
        </form>
        <br>
        <h2>JS原生方法</h2>
        <button id="jsSubmit">js发送</button>
    </section>
    <!-- jqeury -->
    <section>
        <h2>POST</h2>
        <button id="submit">jQuery发送</button>
        <h2>jQuery POST</h2>
        <button id="jqPost">发送</button>
    </section>
{% endhighlight %}
{% highlight js %}
    <script src="http://apps.bdimg.com/libs/jquery/1.6.4/jquery.min.js"></script>
    <script>
        // 采用原生方法
        var jsSubmitBtn = document.getElementById('jsSubmit');
        jsSubmitBtn.onclick = function() {
            var postBody = {
                value: 1
            };
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function() {
                if (xhr.readyState == 4) {
                    if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
                        console.log(xhr.responseText);
                    } else {
                        console.log('unsuccessful:' + xhr.status);
                    }
                }
            };
            xhr.open('get', "http://localhost:8081/formsubmit?first_name=hongyan&last_name=fang", true);
            xhr.send(null);
        }

        // 采用jquery方法
        $(document).ready(function() {
            // 点击jQuery发送
            $('#submit').click(function() {
                var postBody = {
                    value: 1
                };
                httpMethod('http://localhost:8081/postTest', 'post', postBody).then(function(data) {
                    console.log('jQuery', data);
                }, function(data) {
                    console.log('jQuery', data);
                })
                
            });

            function httpMethod(url, method, dataObj) {
                var defer = $.Deferred();
                $.ajax({
                    url: url,
                    type: method,
                    dataType: 'json',
                    data: JSON.stringify(dataObj), // important
                    contentType:'application/json; charset=utf-8', // important
                    success : function(data){
                        defer.resolve(data);//执行状态从"未完成"改为"已完成"
                    },
                    error : function(data){
                        defer.reject(data);
                    }
                })
                return defer.promise();
            }

            $('#jqPost').click(function() {
                var postBody = {
                    'mobile': '13755448866',
                    'code': '123456'
                };
                $.ajax({
                    url: 'http://localhost:8081/postTest',
                    type: 'POST',
                    dataType: 'JSON',
                    contentType: "application/json; charset=utf-8",
                    data: JSON.stringify(postBody)
                })
                .done(function(data) {
                    console.log(data);
                    console.log("success");
                })
                .fail(function(data) {
                    console.log('请重试');
                })  
                
            });
        });
    </script>
{% endhighlight %}

## 测试
在nodexExpress文件夹中按住shift+鼠标右键，打开命令行输入
{% highlight js %}
node serve.js
{% endhighlight %}
运行后台服务，然后打开index.html，就可以在页面上进行简单的操作啦~

![nodeExpress](http://oy41mkgad.bkt.clouddn.com/nodeExpress.png 'nodeExpress')

*每次server.js有更新的话，记得在命令行里重新启动服务~*

源码地址在[这里](https://github.com/fhy-crystal/nodeExpress)

完结撒花❀~