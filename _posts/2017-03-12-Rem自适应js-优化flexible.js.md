---
layout: post
title: "Rem自适应js-优化flexible.js"
date: 2017-03-12
description: "Rem自适应js-优化flexible.js"
tag: rem
---

基于阿里的一个库：lib-flexible.js，里面有一些东西方法和自定义不需要用到的，所以做了一个精简版的，另外还修复了UC浏览器竖屏与横屏转换的BUG。

### js代码

	//designWidth:设计稿的实际宽度值，需要根据实际设置
	//maxWidth:制作稿的最大宽度值，需要根据实际设置
	//这段js的最后面有两个参数记得要设置，一个为设计稿实际宽度，一个为制作稿最大宽度，例如设计稿为750，最大宽度为750，则为(750,750)
	;(function(designWidth, maxWidth) {
		var doc = document,
		win = window,
		docEl = doc.documentElement,
		remStyle = document.createElement("style"),
		tid;

		function refreshRem() {
			var width = docEl.getBoundingClientRect().width;
			maxWidth = maxWidth || 540;
			width>maxWidth && (width=maxWidth);
			var rem = width * 100 / designWidth;
			remStyle.innerHTML = 'html{font-size:' + rem + 'px;}';
		}

		if (docEl.firstElementChild) {
			docEl.firstElementChild.appendChild(remStyle);
		} else {
			var wrap = doc.createElement("div");
			wrap.appendChild(remStyle);
			doc.write(wrap.innerHTML);
			wrap = null;
		}
		//要等 wiewport 设置好后才能执行 refreshRem，不然 refreshRem 会执行2次；
		refreshRem();

		win.addEventListener("resize", function() {
			clearTimeout(tid); //防止执行两次
			tid = setTimeout(refreshRem, 300);
		}, false);

		win.addEventListener("pageshow", function(e) {
			if (e.persisted) { // 浏览器后退的时候重新计算
				clearTimeout(tid);
				tid = setTimeout(refreshRem, 300);
			}
		}, false);

		if (doc.readyState === "complete") {
			doc.body.style.fontSize = "16px";
		} else {
			doc.addEventListener("DOMContentLoaded", function(e) {
				doc.body.style.fontSize = "16px";
			}, false);
		}
	})(750, 750);



### 使用方法：

#### 1.复制上面这段代码到你的页面的头部的script标签的最前面。

#### 2.根据设计稿大小，调整里面的最后两个参数值。

#### 3.使用1rem=100px转换你的设计稿的像素，例如设计稿上某个块是100px*300px,换算成rem则为1rem*3rem。


该代码版本区别于手淘的rem换算方法。使用的是1rem=100px的换算。

假如你有一个块是.box{width:120px;height:80px;} 转为rem则为.box{width:1.2rem; height:.8rem;}



### 基本的HTML模板

你也可以直接复制下面这个基础的HTML模板。

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
	<meta content="yes" name="apple-mobile-web-app-capable">
	<meta content="black" name="apple-mobile-web-app-status-bar-style">
	<meta content="telephone=no" name="format-detection">
	<meta content="email=no" name="format-detection">
	<meta name="description" content="不超过150个字符"/>
	<meta name="keywords" content=""/>
	<meta content="caibaojian" name="author"/>
	<title>前端开发博客</title>
	<link rel="stylesheet" href="base.css">
	<script type="text/javascript">
	//引入该flexible.min.js
	!function(e,t){function n(){var n=l.getBoundingClientRect().width;t=t||540,n>t&&(n=t);var i=100*n/e;r.innerHTML="html{font-size:"+i+"px;}"}var i,d=document,o=window,l=d.documentElement,r=document.createElement("style");if(l.firstElementChild)l.firstElementChild.appendChild(r);else{var a=d.createElement("div");a.appendChild(r),d.write(a.innerHTML),a=null}n(),o.addEventListener("resize",function(){clearTimeout(i),i=setTimeout(n,300)},!1),o.addEventListener("pageshow",function(e){e.persisted&&(clearTimeout(i),i=setTimeout(n,300))},!1),"complete"===d.readyState?d.body.style.fontSize="16px":d.addEventListener("DOMContentLoaded",function(e){d.body.style.fontSize="16px"},!1)}(750,750);
	</script>
	</head>

	<body>
		<!-- 正文 -->
	</body>
	</html>


### base.css

	body,dl,dd,ul,ol,h1,h2,h3,h4,h5,h6,pre,form,input,textarea,p,hr,thead,tbody,tfoot,th,td{margin:0;padding:0;}
	ul,ol{list-style:none;}
	a{text-decoration:none;}
	html{-ms-text-size-adjust:none;-webkit-text-size-adjust:none;text-size-adjust:none;}
	body{line-height:1.5; font-size:14px;}
	body,button,input,select,textarea{font-family:'helvetica neue',tahoma,'hiragino sans gb',stheiti,'wenquanyi micro hei',\5FAE\8F6F\96C5\9ED1,\5B8B\4F53,sans-serif;}
	b,strong{font-weight:bold;}
	i,em{font-style:normal;}
	table{border-collapse:collapse;border-spacing:0;}
	table th,table td{border:1px solid #ddd;padding:5px;}
	table th{font-weight:inherit;border-bottom-width:2px;border-bottom-color:#ccc;}
	img{border:0 none;width:auto\9;max-width:100%;vertical-align:top; height:auto;}
	button,input,select,textarea{font-family:inherit;font-size:100%;margin:0;vertical-align:baseline;}
	button,html input[type="button"],input[type="reset"],input[type="submit"]{-webkit-appearance:button;cursor:pointer;}
	button[disabled],input[disabled]{cursor:default;}
	input[type="checkbox"],input[type="radio"]{box-sizing:border-box;padding:0;}
	input[type="search"]{-webkit-appearance:textfield;-moz-box-sizing:content-box;-webkit-box-sizing:content-box;box-sizing:content-box;}
	input[type="search"]::-webkit-search-decoration{-webkit-appearance:none;}
	input:focus{outline:none;}
	select[size],select[multiple],select[size][multiple]{border:1px solid #AAA;padding:0;}
	article,aside,details,figcaption,figure,footer,header,hgroup,main,nav,section,summary{display:block;}
	audio,canvas,video,progress{display:inline-block;}
	body{background:#fff;}
	input::-webkit-input-speech-button {display: none}
	button,input,textarea{
	-webkit-tap-highlight-color: rgba(0,0,0,0);
	}


### 再次强调一下：

需要根据你的设计稿的大小来调整脚本最后的两个参数。

	})(750, 750);

第一个参数是设计稿的宽度，一般设计稿有640，或者是750，你可以根据实际调整

第二个参数则是设置制作稿的最大宽度，超过750，则以750为最大限制。






文章转自-链接:http://caibaojian.com/simple-flexible.html

转载请注明原地址，冼国基的博客：https://xianguoji.github.io 谢谢！