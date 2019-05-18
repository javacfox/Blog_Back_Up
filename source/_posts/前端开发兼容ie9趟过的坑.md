---
title: 前端开发兼容ie9趟过的坑
date: 2019-05-09 15:29:45
tags: 日常bug记录
declare: true
toc: true
---
# 前言
最近接手了一个银行的项目，合作方要求兼容ie9。之前习惯在chrome浏览器上开发，没有考虑过兼容问题，这次好多在chrome和firefox上都是可以，但是ie9标准版问题千奇百怪，只有你没有想不到的，没有它出不来的！！！

<!--more-->
# 问题集锦
- **文件上传错乱**

根本原因：文件没有清空，上次上传的文件还存在
```js
//之前的写法
$("#id").val("");

//出现错误的问题是ie9不支持，下面是兼容写法
var input = $("#id");
input.replaceWith(input.clone());
```
值得一提的是：上面那种写法在chrome和firefox都是可以的，然而在ie9是行不通的；可是下面那种写法在firefox是无效的，所以最终的解决方案如下：
```js
//navigator.userAgent.toLocaleLowerCase()获取浏览器的版本信息
if(navigator.userAgent.toLocaleLowerCase().indexOf('firefox') != -1){
	document.getElementById('serverCert').value = "";
} else {
	var serverCert = $("#serverCert");
	serverCert.replaceWith(serverCert.clone());		
}
```
最后面这种写法完美兼容了chrome、firefox和ie9


- **选中取消后页面刷新**

根本原因：事件冒泡
```js
//最初添加的兼容ie9的写法
event.preventDefault? event.preventDefault(): event.returnValue = false;
```
这种写法确实解决了ie9的事件冒泡，可是却引出了另外一个bug,火狐识别不了event,于是乎出现了下面这种兼容的写法
```js
//防止冒泡事件的兼容写法
function cancelBubbonEvent(evt){
	if(navigator.userAgent.toLocaleLowerCase().indexOf('firefox') == -1){
		event.preventDefault? event.preventDefault(): event.returnValue = false;
	}else {
		if (evt != undefined) {
			var oEvent = window.event || evt;
			oEvent.preventDefault? oEvent.preventDefault(): oEvent.returnValue = false;
		}
	}
}

//调用时，相应的html页面也需要做一些改变，传入event这个参数
```
之前一直在两者之间切换，问题在两者之间来回变化，之后就用了上面这种写法，完美兼容了ie9,chrome和firefox的表单提交冒泡事件

- **ie9对页面做出修改之后获取的请求结果是没有修改之前的结果**
 
根本原因：ie9对于同一个请求，如果参数没有变化，就默认直接去缓存里面获取先前的请求结果，不会调用后端的接口，所以每次的请求结果都是未修改之前的结果

解决办法：在前端请求方法里添加一个参数datax：new Date(),相应的问题就会解决

# 总结
ie9很容易出现各种各样的问题，还是Chrome好用，支持绝大多数的写法。