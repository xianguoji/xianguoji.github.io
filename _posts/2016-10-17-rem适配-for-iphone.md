---
layout: post
title: "rem适配-for-iphone"
date: 2016-10-17
description: "rem适配-for-iphone"
tag: rem
---

移动端采用 rem 布局方式。通过动态修改 html 的 font-size 实现自适应。

### 实现方式

REM 布局有两种实现方式：CSS 媒介查询和 JavaScript 动态修改。由于 JavaScript 更为灵活，因此现在更多地采用此方式。


我的实现方式是：在 head 标签末加入以下代码：

    <script type="text/javascript">
        !function(){
          var maxWidth=750;
          document.write('<style id="o2HtmlFontSize"></style>');
          var o2_resize=function(){
              var cw,ch;
              if(document&&document.documentElement){
                  cw=document.documentElement.clientWidth,ch=document.documentElement.clientHeight;
              }
              if(!cw||!ch){
                  if(window.localStorage["o2-cw"]&&window.localStorage["o2-ch"]){
                      cw=parseInt(window.localStorage["o2-cw"]),ch=parseInt(window.localStorage["o2-ch"]);
                  }else{
                      chk_cw();//定时检查
                      return ;//出错了
                  }
              }
              var zoom=maxWidth&&maxWidth<cw?maxWidth/375:cw/375,zoomY=ch/603;//由ip6 weChat
              window.localStorage["o2-cw"]=cw,window.localStorage["o2-ch"]=ch;
              //zoom=Math.min(zoom,zoomY);//保证ip6 wechat的显示比率
              window.zoom=window.o2Zoom=zoom;
              document.getElementById("o2HtmlFontSize").innerHTML='html{font-size:'+(zoom*20)+'px;}.o2-zoom,.zoom{zoom:'+(zoom/2)+';}.o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');} .sq_sns_pic_item,.sq_sns_picmod_erea_img{-webkit-transform-origin: 0 0;transform-origin: 0 0;-webkit-transform: scale('+zoom/2+');transform: scale('+zoom/2+');}';
          },
          siv,
          chk_cw=function(){
              if(siv)return ;//已经存在
              siv=setInterval(function(){
                  //定时检查
                  document&&document.documentElement&&document.documentElement.clientWidth&&document.documentElement.clientHeight&&(o2_resize(),clearInterval(siv),siv=undefined);
              },100);
          };
          o2_resize();//立即初始化
          window.addEventListener("resize",o2_resize);
      }();
      </script>

### 从以上代码可得出以下信息：

1.以 iPhone 6 为基准，iPhone 6 的缩放比 zoom 为 1

2.由于只针对移动端，因此最大宽度为768（恰好等于 iPad 的竖屏宽度）

3.通过 document.documentElement.clientWidth 获取视口宽度

4.resize 事件主要考虑横竖屏切换和你在PC上调试时

5.oom 系数是 20。系数决定了在宽度 375 的 iPhone6 下，1 rem 的值是多少 px（20px）。当然如果想过渡到 vw，可以将 zoom 系数设置为 3.75，那么 100rem 就是 375px 了


### 哪些地方要用

由于 rem 布局是相对于视口宽度，因此任何需要根据屏幕大小进行变化的元素（width、height、position 等）都可以用 rem 单位。

但 rem 也有它的缺点——不精细（在下一节阐述），其实这涉及到了浏览器渲染引擎的处理。因此，对于需要精细处理的地方（如通过 CSS 实现的 icon），可以用 px 等绝对单位，然后再通过 transform: scale() 方法等比缩放。

### 字体

那 font-size 是否也要用 rem 单位呢？ 这也是我曾经纠结的地方。如果不等比缩放，对不起设计师，而且对于小屏幕，一些元素内的字体会换行或溢出。当然这可以通过 CSS3 媒介查询解决这种状况。

字体不采用 rem 的好处是：在大屏手机下，能显示更多字体。

看到 网易新闻 和 聚划算 的字体大小都采用 rem 单位，我就不纠结了。当然，也有其它网站是采用绝对单位的，两者没有绝对的对与错，取决于你的实际情况。

### 缺点

#### 小数点（不精细，有间隙）

由于 rem 布局是基于某一设备实现的（目前一般采用 iPhone6），对于 375 倍数宽的设备无疑会拥有最佳的显示效果。而对于非 375 倍数宽的设备，zoom 就可能是拥有除不尽的小数，根元素的字体大小也相应会有小数。而浏览器对小数的处理方式不一致，导致该居中的地方没完全居中，但你又不能为此设置特定样式（如 margin-top: *px;），因为浏览器多如牛毛，这个浏览器微调居中了，而原本居中的浏览器变得不居中了。

对于图标 icon，rem 的不精细导致通过多个元素（伪元素）组合而成的 icon 会形成错位/偏差。因此，在这种情况下，需要权衡是否需要使用 CSS 实现了。




转载请注明原地址，冼国基的博客：https://xianguoji.github.io 谢谢！
