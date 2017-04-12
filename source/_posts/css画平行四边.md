---
title: css画平行四边
date: 2017-04-11 09:20:12
tags: [css, 小技巧, 搬运]
---

### 用css 写一个平行四边形，元素内的其他内容不会跟随变形

方法一 

嵌套元素解决。因为外层元素发生了倾斜，我们可以让容器再多包裹一层元素，反向倾斜来解决这个问题。

```
<div class="button">
  <p class="text">
    hello world
  </p>
</div>
```

```
.button {
  width: 200px;
  height: 50px;
  border: 1px solid blue;
  transform: skew(-45deg);
}
.button .text {
  text-align: center;
  transform: skew(45deg);
}
```
但是如果内层元素太多，就需要一个个去反向变形，显得很冗余。

方法二

伪元素方案。 这个方案在于在背景、边框、变形等样式应用到伪元素上，这样容器内的元素是不会发生变形的。

```
.button {
  width: 200px;
  height: 200px;
  position: relative;
}
.button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  border: 1px solid blue;
  transform: skew(-45deg);
  z-index: -1;
}
.button .text {
  text-align: center;
}
```
