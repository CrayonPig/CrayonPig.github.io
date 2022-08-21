---
title: XShell上传文件到Linux服务器上
date: 2019.12.25 14:00
tags: [Linux, XShell]
---
在学习Linux过程中，我们常常需要将本地文件上传到Linux主机上，这里简单记录下使用Xsheel工具进行文件传输

1：首先连接上一台Linux主机

2：输入rz命令，看是否已经安装了lrzsz，如果没有安装则执行  yum   -y  install  lrzsz命令进行安装。
![image.png](https://upload-images.jianshu.io/upload_images/9056389-53736bf3d69fa1d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3：安装成功后，输入rpm命令确认是否正确安装
![image.png](https://upload-images.jianshu.io/upload_images/9056389-6aa13ed4cdccf5d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4： 使用 rz -y命令进行文件上传，此时会弹出上传的窗口：
![image.png](https://upload-images.jianshu.io/upload_images/9056389-dcac3e4fd8f27020.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5：选择要上传的文件，点击确定即可将本地文件上传到Linux上，如图表示成功上传文件
![image.png](https://upload-images.jianshu.io/upload_images/9056389-ea046957e30f432b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6：使用ls命令可以看到文件已经上传到了当前目录下

![image](https://upload-images.jianshu.io/upload_images/9056389-a7c87b0b4438c560.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
