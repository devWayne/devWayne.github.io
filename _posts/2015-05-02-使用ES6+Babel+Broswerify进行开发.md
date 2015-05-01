---
layout: post
title: 使用ES6+Babel+Broswerify进行开发
comments: true
tags: 我的笔记
---

## Babel

Babel 原名是6to5，是目前社区上认可度最高的一款ES6编译器。   

支持Cli调用方式和构建工具调用方式

控制台调用方式：
{% highlight sh%}
$ babel src/es6 --out-dir dist/es6 --source-maps
{% endhighlight %}

babel支持生成source-maps极大得方便了调试

## Broswerify

Broswerify是一个基于CommonJS模块标准的打包工具，能够将模块依赖合并到一个文件中，从而在浏览器中运行使用CommonJS模块方式编写的Js代码

Broswerify本身是一个Cli工具，也提供了大量的API接口，使得它可以在NodeJS的环境中可以进行二次开发。

利用Broswerify来编写模块，可以使模块同时在CommonJS和浏览器中运行，如[MinPanel](https://github.com/devWayne/MinPanel)   

控制台调用方式：
{% highlight sh%}
$ browserify index.js  > dist/bundle.js
{% endhighlight %}


## Babelify

Babelify是一个transform工具将babel编译后得文件转换成能够让Broswerify识别的文件

控制台调用方式：
{% highlight sh%}
$ browserify index.js -t babelify --outfile bundle.js
{% endhighlight %}


使用gulp:
{% highlight javascript%}
gulp.task('browserify-es6', function() {
    var b = browserify();
    b.add('./src/index.es6');
    return b
    	.transform(babelify)
	.bundle()
        .pipe(source('index.js'))
        .pipe(gulp.dest(dirs.dist))
});
{% endhighlight %}


## 小结

利用ES6+Babel+Broswerify可以方便得编写基于ES6和CommonJS模块标准模块   

EMCAScript6给Javascript开发带来了许多全新的概念和便利。   

[Rythme](https://github.com/devWayne/Rythme)是一个长期的项目，将以前所开发过得模块用es6进行重写，一起来感受ES6带来得全新开发体验吧！

