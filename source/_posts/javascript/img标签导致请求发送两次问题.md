---
title: <img>标签导致请求发送两次问题
date: 2021.01.13 20:48
tags: [H5, js, 统计]

---

## 问题描述

项目中有个需求是统计用户浏览时长，我监听pagehide事件的时候，创建一个img，给src赋值，利用get请求去完成这个操作，代码如下

```javascript
		const objStr = this.qsStringify(obj);
    let dataScript = document.createElement('img');
    dataScript.src = getWechatReadLog + '?' + objStr;
    dataScript.style.position = 'fixed';
    dataScript.style.zIndex = '-999';
    dataScript.style.bottom = '0';
    dataScript.style.opacity = '0';
    // 隐藏样式
    document.body.appendChild(dataScript);
```

后续后端同学反馈说后端日志收到两个一模一样的请求，请求时间都是一样的，怀疑前端发了两遍。前端通过代理查看network中的请求是正常的只有一次。

在网上查询，也只是查到[这样的](https://blog.csdn.net/u013185616/article/details/52921298)

![image-20210413153829996](https://img.hellohl.com/image-20210413153829996.png)

虽然没有查到明确答案，但有句话给了我一些提示：

> **在img 对象的src 属性是空字符串("")的时候，浏览器认为这是一个缺省值，值的内容为当前网页的路径。浏览器会用当前路径进行再一次载入，并把其内容作为图像的二进制内 容并试图显示**

于是我尝试性的注释掉

*document.body.appendChild(dataScript);*

就只向后端发送一次请求了

## 总结

img对象会加载src中的内容，插入到页面中后又会去加载一次，两次加载一起发生，导致同时出现了两种请求
