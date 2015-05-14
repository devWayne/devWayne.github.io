---
layout: post
title: 用EMCAScript6来开发模块
comments: true
tags: 我的笔记 我的作品
---

[Rythme](https://github.com/devWayne/Rythme)是一个长期的项目，将以前所开发过的常用模块用es6进行重写，在重写的过程中，慢慢感受ES6带来得全新的JS开发体验。

## 关于如何使用ES6进行开发

[参照本文:使用ES6+Babel+Broswerify进行开发](http://kylar.cn/2015/02/02/%E4%BD%BF%E7%94%A8ES6+Babel+Broswerify%E8%BF%9B%E8%A1%8C%E5%BC%80%E5%8F%91)

## ES6 Class
ES6引入了Class的概念，虽然其本质还只是一个语法糖，但是对于JS模块的开发来说，终于可以像写java类一样来开发，可以说非常值得一式。

来看以下一个场景：   

![网页截图]({{ site.url }}/assets/用EMCAScript6来开发模块/class.jpg)

这张图是否看起来很有java中类图的感觉呢?
{% highlight javascript %}

//父类
export default class Popup {

	//构造函数
	constructor(){
		this.opt=DEFAULT;
		this.OL=this.createOL();
		this.EL;
	}

	remove(){
		this.OL.hide();
		this.EL.parentNode.removeChild(this.EL);
	}

	bindEvent(EL){
		EL.addEventListener('click',e=> this.remove(),false);	
	}
	createOL(){
		return this.OL ? this.OL : new Overlay(2000,true);
	}
}

{% endhighlight %}

{% highlight javascript %}

//子类
export default class Confirm extends Popup{

	constructor(opt){
		//子类必须在constructor方法中调用super方法
		super();
		this.opt=extend(DEFAULT,opt);
		let dom=document.createElement('div');
		dom.innerHTML=this.buildHtml()
		this.EL=this.opt.container.appendChild(dom);
		//调用父类方法
		this.bindEvent(this.EL);
	}

	buildHtml(){
		return '<div class="confirm_alert"></div>'
	}
}
{% endhighlight %}


	
