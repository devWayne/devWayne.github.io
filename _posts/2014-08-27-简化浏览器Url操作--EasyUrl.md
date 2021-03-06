---
layout: post
title: 简化浏览器Url操作--EasyUrl
comments: true
tags: 我的作品
---

项目地址   
[git repo](https://github.com/devWayne/EasyUrl.js)

![Url解析]({{ site.url }}/assets/简化浏览器Url操作--EasyUrl/url.jpg)

##Url成分

- location.protocol
- location.host
- location.search
- location.hash
- location.origin(在chrome中支持)


##Url Hash操作 

Hash值常用于网页的定位，在单页应用框架中亦可以当做实现路由功能的实现   
Hash值是带在`#`后得一个字符串，所以在操作中相对的简单,API模仿jQuery中得attr()  

{% highlight javascript %}
var myurl = new URL();
//获取hash
var newUrl = myurl.hash();
//设置hash
var newUrl = myurl.hash('newHash');
{% endhighlight %}

##Url 参数操作
前端开发中经常会遇到对当前Url进行操作。
可以将操作分为四大类，类似于数据库操作的'CRUD'   
也就是新建(create),查找(read),更新(update),删除(delete)   


###查找(read)

在使用的过程中将参数转换成对象可以更加方便进行操作   
参数在对象和数组之间的互相转换   
   
{% highlight javascript %}
function obj2arr(obj){
    var _arr=[];
    Object.keys(obj).forEach(function(v,idx){
    	_arr.push(v+'='+obj[v].toString());
    })
    return _arr;
}
{% endhighlight %}

{% highlight javascript %}
function arr2obj(arr){
    var _obj={};
    arr.forEach(function(v,idx){
    	_obj[v.split('=')[0]]=v.split('=')[1];
    });
    return _obj;
}
{% endhighlight %}
使用方式   

{% highlight javascript %}
var myurl = new URL();
//获取Object类型
var paramsObj = myurl.readParams();
//获取Array类型
var paramsArray = myurl.readParams({type:'Array'});

{% endhighlight %}

###更新(update)

在前端控制路由的时候，常常会需要配置跳转链接,这里提供一个较为便捷的方法，可以快速将参数转成URL进行操作      

使用方式   

{% highlight javascript %}
var myurl = new URL();
//新的URL参数
var _data = {};
//如果参数重复，用新的参数替换
var newUrl = myurl.updateParams({data:_data});
//如果参数重复，保留旧得参数
var newUrl = myurl.readParams({override:false,data:_data});
{% endhighlight %}

###创建(create)

在创建一个Url之前首先会进行判断，若Url已经包含参数，则返回空对象，建议使用更新方法

使用方式   

{% highlight javascript %}
var myurl = new URL();
var _data = {};
var newUrl = myurl.createParams(_data);
{% endhighlight %}

###删除(delete)

删除Url中得参数

使用方式   

{% highlight javascript %}
var myurl = new URL();
var _data = {};
var newUrl = myurl.deleteParams(_data);
{% endhighlight %}


