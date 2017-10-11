---
title: 移动端rem布局方法
date: 2017-06-08 17:13:47
tags: [笔记]
---

用rem布局移动端有好多方法，html元素字体大小 = document根节点(html)宽度 * 100 / 设计图宽度
计算html元素的font-size,使1rem等于100px,方便快速开发 

#### 方法1
font.js
```
//控制字体
var calculate_size = function() {
	var BASE_FONT_SIZE = 100;
	var docEl = document.documentElement,
		clientWidth = docEl.clientWidth;
	if (!clientWidth) return;
	docEl.style.fontSize = BASE_FONT_SIZE * (clientWidth / 640) + 'px';
};
// 如果浏览器不支持addEventListener，则中止
if (document.addEventListener) {
	var resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
	window.addEventListener(resizeEvt, calculate_size, false);
	document.addEventListener('DOMContentLoaded', calculate_size, false);
	calculate_size();
}

```

#### 方法2
```
(function(win) {
    function remChange() {
        document.documentElement.style.fontSize = document.documentElement.clientWidth * 20 / 375 + 'px';
    }
    remChange();
    win.addEventListener('resize', remChange, false);
    document.addEventListener('DOMContentLoaded', remChange, false);
})(window);
```

#### 方法3
rem.js
```
(function(doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function() {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            docEl.style.fontSize = 20 * (clientWidth / 375) + 'px';
        };
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);
```

#### 方法4
淘宝写法处理1pxbug，不需要写 `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0,user-scalable=no">`
```
(function(doc, win) {
    var radio = win.devicePixelRatio || 1,
        scale = 1 / radio,
        docEl = doc.documentElement,
        meta = document.createElement("meta"),
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function() {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            docEl.style.fontSize = 20 * (clientWidth / 375) + 'px';
        };
    recalc();
    docEl.setAttribute("data-dpr", radio);
    meta.setAttribute("name", "viewport")
    meta.setAttribute("content", "width=device-width,initial-scale=" + scale + ", maximum-scale=" + scale + ", minimum-scale=" + scale + ", user-scalable=no")
    if (docEl.firstElementChild) docEl.firstElementChild.appendChild(meta);

    if (!doc.addEventListener) return;

    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);

```
#### 方法5

网友推荐
```
window.onload = function(){
    /*720代表设计师给的设计稿的宽度，你的设计稿是多少，就写多少;100代表换算比例，这里写100是
      为了以后好算,比如，你测量的一个宽度是100px,就可以写为1rem,以及1px=0.01rem等等*/
    getRem(720,100)
};
window.onresize = function(){
    getRem(720,100)
};
function getRem(pwidth,prem){
    var html = document.getElementsByTagName("html")[0];
    var oWidth = document.body.clientWidth || document.documentElement.clientWidth;
    html.style.fontSize = oWidth/pwidth*prem + "px";
}
```

相关设置
1. 新增最大最小宽度，解决平板或手机横屏时体验不佳的问题 
```
body { 
    max-width: 6.4rem; // 设计图宽度为640px时为6.4rem ,750时为7.5rem ，以此类推 
    min-width: 3.2rem;
    margin: 0 auto; 
} 
```


