---
layout: post
title: 跨终端代理工具-HttpProxyServer
comments: true
tags: 我的作品
---

[项目GIT地址](https://github.com/devWayne/hps)

## 线上文件代理

在前端开发的过程中，经经常会碰到需要对生产环境的代码进行调试的场景，而一般生产环境中的静态资源都是经过压缩的，会给调试带来许多不便。   

![网页截图]({{ site.url }}/assets/跨终端代理工具-HttpProxyServer/min-js.jpg)


在windows系统中有一个非常好用的代理工具Fidder，可以帮助我们使用本地源码文件替换线上生产环境的文件，但是由于是一款windows软件，在mac和Linux下无法使用。   

基于这种现状，利用nodejs的跨平台特性，开发了HttpProxyServer来实现文件代理的功能，其基本原理是利用nodejs的http模块创建了一个代理服务器，配合chrome的switchsharp将http请求转入代理服务器内。   

代理服务捕获所有的Http请求，根据HttpProxyServer的config文件来判断是否替换为本地文件。    

对于移动端开发来说，同样适用，不同的是通过连接代理wifi将请求转入代服务器内，下文会有详细介绍。

## 使用方式

#### 全局安装hps

{% highlight sh %}

$ npm i -g hps

{% endhighlight %} 

#### 找到需要代理文件的url

![网页截图]({{ site.url }}/assets/跨终端代理工具-HttpProxyServer/min-url.jpg)

#### 使用SwitchySharp进行浏览器代理

SwitchySharp是一款chrome插件，也是目前市面上最受哦欢迎的浏览器代理工具。   
通过配置可以将浏览器的请求全部转发到设定的端口上，在这里设为7000.

![网页截图]({{ site.url }}/assets/跨终端代理工具-HttpProxyServer/switchsharp.jpg)

#### 编写配置文件

`map`变量是一个数组，里面存放需要代理的线上文件和本地文件路径，如：
{% highlight javascript %}

exports.port=7000;

exports.map=[
	['http://t2.s2.dpfile.com/t/jsnew/app/newhome/page/movie-list.min.8a5d9f0637b67d3a9aac1eb748d9443d.js','/Users/wayne/code/dianping/tuangou-home-static/js/page/movie-list.js']
];

{% endhighlight %} 

这里的port对应的就是浏览器转发的端口。将该文件保存为`config.js`。

#### 在Shell中运行指令

第二个参数表示config文件的路径
{% highlight sh %}

$ hps start ./config.js

{% endhighlight %} 


#### 刷新页面

刷新页面后，可以看到控制台输出

![网页截图]({{ site.url }}/assets/跨终端代理工具-HttpProxyServer/shell.jpg)

这时在浏览器中再次找到这个文件时，可以看到线上文件已经替换为本地文件了。

![网页截图]({{ site.url }}/assets/跨终端代理工具-HttpProxyServer/debug-js.jpg)

