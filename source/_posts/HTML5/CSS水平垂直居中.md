---
title: CSS水平垂直居中
date: 2018.04.08 11:36
tags: [H5, css]
---
水平居中之行内元素解决方案
      只需要把行内元素包裹在一个属性display为block的父层元素中，并且把父层元素添加如下属性即可：
``` javascript
.parent {
  text-align:center;
}
``` 
水平居中：块状元素解决方案
``` javascript
.item {
  /* 这里可以设置顶端外边距 */ 
  margin: 10px auto;
}
``` 

水平居中：多个块状元素解决方案
       将元素的display属性设置为inline-block，并且把父元素的text-align属性设置为center即可:

``` javascript
.parent { 
  text-align:center;
}
``` 

水平居中：多个块状元素解决方案 (使用flexbox布局实现)
       使用flexbox布局，只需要把待处理的块状元素的父元素添加属性display:flex及justify-content:center即可:
``` javascript
.parent {    
  display:flex;    
  justify-content:center; 
}
``` 

垂直居中：单行的行内元素解决方案
``` javascript
.parent {    
  background: #222;    
  height: 200px;
  }
``` 

/* 以下代码中，将a元素的height和line-height设置的和父元素一样高度即可实现垂直居中 */

``` javascript
a {    
  height: 200px;    
  line-height:200px;    
  color: #FFF;
  }
``` 

垂直居中：多行的行内元素解决方案
       组合使用display:table-cell和vertical-align:middle属性来定义需要居中的元素的父容器元素生成效果，如下：

``` javascript
.parent {    
  background: #222;    
  width: 300px;    
  height: 300px;   
  /* 以下属性垂直居中 */   
  display: table-cell;   
  vertical-align:middle; 
  }
``` 

垂直居中：已知高度的块状元素解决方案
``` javascript
.item{    
  top: 50%;    
  margin-top: -50px; 
  /* margin-top值为自身高度的一半 */   
  position: absolute;    
  padding:0;  
}
``` 

垂直居中：未知高度的块状元素解决方案
``` javascript
.item{    
  top: 50%;    
  position: absolute;    
  transform: translateY(-50%); 
  /* 使用css3的transform来实现 */   
}
``` 

水平垂直居中：已知高度和宽度的元素解决方案1
       这是一种不常见的居中方法，可自适应，比方案2更智能，如下：
``` javascript
.item{    
  position: absolute;    
  margin:auto;    
  left:0;    
  top:0;    
  right:0;    
  bottom:0;
}
``` 

水平垂直居中：已知高度和宽度的元素解决方案2
``` javascript
.item{    
  position: absolute;    
  margin:auto;    
  left:0;    
  top:0;    
  right:0;    
  bottom:0;
  }
``` 

水平垂直居中：未知高度和宽度元素解决方案
``` javascript
.item{    
  position: absolute;    
  top: 50%;    
  left: 50%;    
  transform: translate(-50%, -50%);  
  /* 使用css3的transform来实现 */    
}
``` 

水平垂直居中：使用flex布局实现
``` javascript
.parent{    
  display: flex;    
  justify-content:center;    
  align-items: center;   

  /* 注意这里需要设置高度来查看垂直居中效果 */   
  background: #AAA;    
  height: 300px;   
}
``` 

