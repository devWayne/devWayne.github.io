---
layout: default
title: 简化浏览器Url操作--EasyUrl
comments: true
tags: works
---
#简化浏览器Url操作--EasyUrl
项目地址   
[git repo](https://github.com/devWayne/EasyUrl.js)

![Url解析]({{ site.url }}/assets/简化浏览器Url操作--EasyUrl/url.jpg)

##Url成分

- location.protocol
- location.host
- location.search
- location.hash
- location.origin(在chrome中支持)


##Url参数
前端开发中经常会遇到对当前Url进行操作。
可以将操作分为四大类，类似于数据库操作的'CRUD'   
也就是新建(create),查找(read),更新(update),删除(delete)   


###查找(read)

在使用的过程中将参数转换成对象可以更加方便进行操作   
参数在对象和数组之间的互相转换   
```
function obj2arr(obj){
    var _arr=[];
    Object.keys(obj).forEach(function(v,idx){
    	_arr.push(v+'='+obj[v].toString());
    })
    return _arr;
}
```
```
function arr2obj(arr){
    var _obj={};
    arr.forEach(function(v,idx){
    	_obj[v.split('=')[0]]=v.split('=')[1];
    });
    return _obj;
}
```

使用方式   
```
var myurl = new URL();
//获取Object类型
var paramsObj = myurl.readParams();
//获取Array类型
var paramsArray = myurl.readParams({type:'Array'});

```

###更新(update)

在前端控制路由的时候，常常会需要配置跳转链接,这里提供一个较为便捷的方法，可以快速将参数转成URL进行操作      

使用方式  
```
var myurl = new URL();
//新的URL参数
var _data = {};
//如果参数重复，用新的参数替换
var paramsObj = myurl.updateParams({data:_data});
//如果参数重复，保留旧得参数
var paramsArray = myurl.readParams({override:false,data:_data});
```
