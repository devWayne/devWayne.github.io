---
layout: default
title: iOS position fixed 在输入框获得焦点时定位失效
---
# iOS position fixed 在输入框获得焦点时定位失效


##问题
在ios（实测IOS8仍然存在该问题）中，如果某一DOM节点的postion定位为fixed，并且该节点中的某个子节点会唤起虚拟键盘的时候，Position:fixed的定位会被打破，造成下图中得效果：

![ip4-1](({{ site.url }}/assets/IOS-position-fixed/ip4-1.png)
![ip4-2](({{ site.url }}/assets/IOS-position-fixed/ip4-2.png)


这一类的场景在社交功能类得App中可以说非常的常见，
通过现象可以推测在输入框获得焦点的时候，footer的位置被居中了，并且可以进行滚动。
这个BUG是IOS webkit的一个经典BUG，网上也可以找到较多的解决方案。简而言之，在调起虚拟键盘的时候，页面的流被打破了，可以通过手工来设定改变这个时候的定位来解决这个问题。

以下是一种成本最低，且效果最佳的方案(不会引发额外的BUG)。

##解决方案：
```
<!DOCTYPE html>
    <head>
	<!-- head stuffs -->
    </head>
    <body>
        <div class="header">A positon :fixed Header<input /></div>
        <div class="container">
	<!-- main part -->
        </div>
        <div class="footer">A positon :fixed Footer<input /></div>
    </body>
</html>
```
```
    $(document)
    .on('focus', 'input', function(e) {
        $('.header').addClass('fixfixed');
 	$('.footer').addClass('fixfixed');
    })
    .on('blur', 'input', function(e) {
        $('.header').removeClass('fixfixed');
	$('.footer').removeClass('fixfixed');
    });
```
```
.header { 
    position: fixed; 
    height: 100px; 
    width: 100%; 
    top: 0; 
    z-index: 1000; 
} 
.footer { 
    position: fixed; 
    height: 30px; 
    width: 100%; 
    bottom: 0; 
    z-index: 1000; 
} 

//Hack
.fixfixed, 
.fixfixed{ 
    position: absolute; 
} 
```
