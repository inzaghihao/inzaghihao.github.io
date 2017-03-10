---
title: flex布局笔记
date: 2017-03-10 18:57:43
tags: [html, css]
categories: html,css
---

废话不多说！直接上手


> 采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。

```
<div class="p">
	<div class="s">
		么么哒
	</div>
</div>
```

```
	.p{display: -webkit-flex; /* Safari */ display: flex;} 		//容器属性
	.s{flex:1;} 			//项目属性
```

## Flex容器属性（设置在父元素上）

### flex-direction 子元素沿着主轴（Main Axis）的排列方向

- flex-direction:row	水平排列，从左至右(默认)
- flex-direction:column	垂直排列
- flex-direction:row-reverse	水平排列，从右至左
- flex-direction:column-reverse	垂直排列，从下至上


### justify-content 子水平对齐方式

- justify-content: flex-start;
- justify-content: flex-end;
- justify-content: center;
- justify-content: space-between;	（两端对齐）
- justify-content: space-around; (让每个Flex项目具有相同的空间)


### flex-wrap  子元素自动换行

- flex-wrap:nowrap 默认不换行
- flex-wrap:wrap 自动换行
- flex-wrap:wrap-reverse 换行反向排列


### flex-flow flex-direction 和 flex-wrap两个属性的速记属性。

- flex-flow: row wrap;


### align-items 垂直对齐方式

- align-items:stretch;	让所有的Flex项目高度和Flex容器高度一样（默认值）
- align-items:flex-start;	顶部对齐
- align-items:flex-end;	底部对齐
- align-items:center; 	垂直居中对齐
- align-items:baseline; 	自己的基线对齐。


### align-content 多项目垂直对齐方式

- align-content:stretch;	让所有的Flex项目高度和Flex容器高度一样（默认值）
- align-content:flex-start;	顶部对齐
- align-content:flex-end;	底部对齐
- align-content:center; 	垂直居中对齐


## Flex项目属性

### order 子元素顺序

- Flex项目会根据order值重新排序。从底到高。


### flex	是flex-grow、flex-shrink和flex-basis 三个属性的速记(简写)

1. flex-grow	填充可用空间
2. flex-shrink	如果有宽度的话 到了宽度限制的开关(收缩开关)
3. flex-basis	指定Flex项目的初始大小（默认值auto）


### align-self	更进一步让我们更好地控制弹性项目

- align-self:auto	设置为父元素的 align-items值
- align-self:flex-end	项目属性对齐到底部
- align-self:center	居中对齐
- align-self:baseline	基准线对齐


未完待续..
