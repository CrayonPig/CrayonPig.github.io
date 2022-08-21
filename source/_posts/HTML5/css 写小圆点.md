---
title: css 写小圆点
date: 2020.01.16 10:56
tags: [H5, css]
---
有需求要做小圆点，我初始写法如下
```
.point{
  width: 0.08rem;
  height: 0.08rem;
  position: absolute;
  right: 0;
  top: 0;
  background: #FD3509;
  border-radius: 50%;
}
```
浏览器上无问题，真机实测发现小圆点变成了小方点，后改为如下
```
.point{
  width: 0.08rem;
  height: 0.08rem;
  border: 0.01rem solid #FD3509;
  position: absolute;
  right: 0;
  top: 0rem;
  background: #FD3509;
  border-radius: 50%;
  z-index: 1;
}
```
增加border属性