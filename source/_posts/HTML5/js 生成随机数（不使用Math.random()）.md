---
title: js 生成随机数(不使用Math.random())
date:  2018.10.18 14:20
tags: [H5, js, 随机数]
---
朋友问了我一道面试题，如下：
>实现一个返回随机数的函数，要求如下：
>1.不可以使用Math.random()
>2.有一个函数 Rand() 返回1-5的随机数

想了很久除了获取时间的毫秒数外，我没法用一个固定的代码区生成会变动的数据，但根据毫秒数怎么获取一个足够随机的数值我不知道怎么处理了，多方查证下找到如下代码：

```javascript
    rand = (function(){
      var today = new Date(); 
      var seed = today.getTime();
      function rnd(){
        seed = ( seed * 9301 + 49297 ) % 233280;
        return seed / ( 233280.0 );
      };
      return function rand(number){
        return Math.ceil(rnd() * number);
      };
    })();
```

至于为什么是9301、49297、233280这几个数字，有兴趣的朋友可以去[知乎](https://www.zhihu.com/question/22818104)


我的理解是，利用这个几个数字将毫秒数变得足够随机，然后使用进一法将其变为整数，至于为何是进一法，因为题目要求是1-n，而不是0-n