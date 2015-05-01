---
layout: post
title: AMD&CommonJS模块标准
comments: true
tags: 我的笔记
---


##CommonJS模块标准

CommonJS
{% highlight javascript %}
// someModule.js
exports.doSomething = function() { return "foo"; };

//otherModule.js
var someModule = require('someModule'); // in the vein of node    
exports.doSomethingElse = function() { return someModule.doSomething() + "bar"; };
{% endhighlight %}

### 选择CommonJS模块标准作为模块加载的理由

兼容NodejS模块标准，有大量活跃的社区支持模块


##AMD标准

AMD标准本是CommonJS中模块标准的草案，不过最终并没有被采纳，最后AMD自身独立了出来。   
AMD标准有2个关键的概念，分别是`define`和`require`

###define

使用方式:
{% highlight javascript %}
define(
    module_id /*optional*/, 
    [dependencies] /*optional*/, 
    definition function /*function for instantiating the module or object*/
);
{% endhighlight %}

`define`一个模块实例:
{% highlight javascript %}
define('myModule', 
    ['foo', 'bar'], 
    // module definition function
    // dependencies (foo and bar) are mapped to function parameters
    function ( foo, bar ) {
        // return a value that defines the module export
        // (i.e the functionality we want to expose for consumption)
    
        // create your module here
        var myModule = {
            doStuff:function(){
                console.log('Yay! Stuff');
            }
        }
 
        return myModule;
});
{% endhighlight %}

###require

{% highlight javascript %}
require(['foo', 'bar'], function ( foo, bar ) {
        // rest of your code here
        foo.doSomething();
});
{% endhighlight %}


###完整的一个实例
{% highlight javascript %}

define(['lib/Deferred'], function( Deferred ){
    var defer = new Deferred(); 
    require(['lib/templates/?index.html','lib/data/?stats'],
        function( template, data ){
            defer.resolve({ template: template, data:data });
        }
    );
    return defer.promise();
});
{% endhighlight %}

### 选择AMD作为模块加载的理由

- 提供一个清晰的接口来定义和加载模块
- 每一个模块都是一个独立的个体，避免滥用全局命名空间
- 懒加载机制，不到必要时不主动加载脚本

### AMD标准的不足之处

- 与现有的模块无法兼容，需要对每一个模块进行重定义
