---
title: 用Markdown写Markdown笔记
date: 2017-03-07 18:24:09
tags: [markdown]
categories: markdown笔记
---

### 标题

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

### 列表

列表的显示只需要在文字前加上 - 或 * 即可变为无序列表，有序列表则直接在文字前加1. 2. 3. 符号要和文字之间加上一个字符的空格。

#### 无序列表
* 1
* 2
* 3

- 1
- 2
- 3

#### 有序列表
1. 我
2. 爱
3. 你

### 引用

如果你需要引用一小段别处的句子，那么就要用引用的格式 加大于号>。

>例如这样

### 图片与链接

插入链接与插入图片的语法很像，区别在一个 !号

图片为：``![](){ImgCap}{/ImgCap}``

链接为：``[]()``

![](https://cdn.sspai.com/attachment/origin/2014/04/15/69495.jpg)

### 粗体与斜体

Markdown 的粗体和斜体也非常简单，用两个星包含一段文本就是粗体的语法，用一个星包含一段文本就是斜体的语法。

例如：**这里是粗体**    *这里是斜体*

### 表格

略。。

### 代码框

只需要用两个 ` 把中间的代码包裹起来

``
	hello markdown!
``

### 分割线

分割线的语法只需要三个 * 号，例如

*** 
*** 我是快乐的分割线
*** 

### 视频

目前只支持HTML的video标签，例如：
```
<video id="video" controls="" preload="none" poster="http://media.w3.org/2010/05/sintel/poster.png">
  <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
  <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" type="video/webm">
  <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" type="video/ogg">
  <p>Your user agent does not support the HTML5 Video element.</p>
</video>
```
<video id="video" controls="" preload="none" poster="http://media.w3.org/2010/05/sintel/poster.png">
  <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
  <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" type="video/webm">
  <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" type="video/ogg">
  <p>Your user agent does not support the HTML5 Video element.</p>
</video>

