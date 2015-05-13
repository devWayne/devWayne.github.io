---
layout: post
title: MinRequest-轻量化jsonp跨域实现
comments: true
categories: Jekyll Update
tags: 我的作品 我的笔记
---

## 关于跨域

在运营活动类得项目开发中，通常使用静态FTP服务器来发布上线。如果FTP服务器配置的是单独的域名的话，很可能发生和API接口不在一个子域名下，这个时候就有可能发生跨域的现象。

附：跨域对照表：
![跨域对照表]({{ site.url }}/assets/MinRequest-轻量化jsonp跨域实现/corstable.png)

当发生跨域的时候，在前端有以下几种常用解决方案：

1. JSONP

2. postMessage API [参见本文](http://kylar.cn/2015/04/07/iframe%E8%B7%A8%E5%9F%9F%E5%8F%82%E6%95%B0%E4%BC%A0%E9%80%92%E6%96%B9%E5%BC%8F.html)

3. FLASH

4. 设置document.domain (只适用于一级域名相同，二级域名不同得情况下）

5. window.name

6. location.hash   

在条件允许的情况下，jsonp是一种使用最广泛，在有库类得情况下，改动最小的方式。

## JSONP原理&MinRequest实现

[MinRequest源码](https://github.com/devWayne/MinRequest)

### API设计

当判断`dataType`为`jsonp`的时候，跳出正常得XHR流程，调用jsonp方法
{% highlight javascript %}
    if (option.dataType == 'jsonp') {
        jsonp(option);
	return;
    }
{% endhighlight %} 

### 拼装请求url，在请求的URL后面加上一个参数```callback=xxx```

{% highlight javascript %}
    //id为回调的方法名
    var url = option.url;    var id='jsonp'+(count++);
    //参数名默认取callback，根据后端实际定义情况而变
    var jsonp = opts.jsonp || 'callback';
    url += (url.indexOf('?') ? '&' : '?') + jsonp + '=' + encodeURIComponent(id);
    url = url.replace('?&', '?');
{% endhighlight %} 


### 创建一个新的`<script>`TAG在节点尾部并立即执行

{% highlight javascript %}
	// create script
	script = document.createElement('script');
	script.src = url;
	document.getElementsByTagName('body')[0].appendChild(script);
{% endhighlight %} 


### 获得后端返回后，执行成功回调
{% highlight javascript %}
    window[id]=function(data){
    	cleanup();
	if(option.success)option.success(data);
    }
{% endhighlight %} 

### 清除`<script>`TAG，在成功回调或者超时后，调用清除函数

{% highlight javascript %}
    var timeout = option.timeout ? option.timeout : 60000;
	
    setTimeout(function(){
    	 cleanup();
	 if (option.success) option.success(new Error('Timeout'));
    });

    function cleanup() {
        if (script.parentNode)  document.getElementsByTagName('body')[0].removeChild(script);
        window[id] = function(){};
        if (timer) clearTimeout(timer);
    }
{% endhighlight %} 

