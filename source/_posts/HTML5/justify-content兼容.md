---
title: justify-content:space-evenly 兼容
date: 2020.01.16 10:52
tags: [H5, css]
---
justify-content 属性定义了浏览器如何分配顺着父容器主轴的弹性元素之间及其周围的空间。

space-evenly: 均匀排列每个元素,每个元素之间的间隔相等

space-between: 均匀排列每个元素,首个元素放置于起点,末尾元素放置于终点
兼容解决如下：
``` javascript
box{
      display: flex;
      flex-flow: row nowrap;
      align-items: center;
      justify-content: space-between;
       //justify-content: space-evenly;
      &:before,
      &:after {
          content: '';
          display: block;
    }
}
```