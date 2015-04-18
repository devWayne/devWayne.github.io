---
layout: default
title: 跨终端代理工具-HttpProxyServer
comments: true
tags: notes
---

#跨终端代理工具-HttpProxyServer

[项目GIT地址](https://github.com/devWayne/hps)

在前端开发的过程中，经经常会碰到需要对生产环境的代码进行调试的场景，而一般生产环境中的静态资源都是经过压缩的，会给调试带来许多不便。   

在windows系统中有一个非常好用的代理工具Fidder，可以帮助我们使用本地源码文件替换线上生产环境的文件，但是由于是一款windows软件，在mac和Linux下无法使用。   

基于这种现状，利用nodejs的跨平台特性，开发了HttpProxyServer来实现文件代理的功能，其基本原理是利用nodejs的http模块创建了一个代理服务器，配合chrome的switchsharp将http请求转入代理服务器内。   

代理服务捕获所有的Http请求，根据HttpProxyServer的config文件来判断是否替换为本地文件。    

对于移动端开发来说，同样适用，不同的是通过连接代理wifi将请求转入代服务器内，下文会有详细介绍。