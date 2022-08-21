---
title: 玩转ubuntu——nginx篇
date: 2018.04.09 17:06
tags: [ubuntu, nginx]
---
一直想搭建自己的网站，但由于自己拖延症晚期，导致一直拖。今天朋友问我怎么配置自己的服务器，干脆趁着自己还有兴趣，从零配置下服务器。

我服务器用的是ubuntu16.04 64位，因为最近在自学python3，ubuntu16.04自带python3。

首先分析下，一般配置服务器就是安装nginx，mysql以及相关的后台语言，考虑到大部分朋友上手采用的是php，那我就以php为例。

我服务器用的是阿里云的，我利用PUTTY软件进行远程连接。
![putty英文版界面](https://upload-images.jianshu.io/upload_images/9056389-efc20d718bf2c63a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![询问](https://upload-images.jianshu.io/upload_images/9056389-e2ae73c31fb0ae96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


回车后会出现这个界面，大意是询问这个服务器你第一次连接，你要注意他的安全之类的，选择是即可

中文界面如下：
![询问](https://upload-images.jianshu.io/upload_images/9056389-eaa3e1fd27d556f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![登录界面](https://upload-images.jianshu.io/upload_images/9056389-a4420165b2f61988.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


选择是后进入登录界面，输入你的用户名和密码，注意这里的秘密是不显示字符的，当初不知道这个事情导致我一直觉得是服务器有问题。输入密码后还是回车。

![登录成功](http://upload-images.jianshu.io/upload_images/9056389-843065fc874bb8e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这就是登录成功后的界面了，如果让你重新输入密码，那你就得看看是不是登录密码错误或者用户名错了。有个小技巧就是你在其他地方输入密码后复制过来，服务器里的粘贴不是我们平时用的crtl+v，是单击鼠标右键。

登录成功后，我们首先开始装nginx,依次输入如下命令
```
sudo  apt  update   #更新源，sudo为获取最高权限，有时会让你再次输入服务器密码

sudo  apt  install  nginx   #安装ngxin，会提示你输入y/n继续或者取消，当然是输入y

whereis nginx  #查看nginx安装路径
```
![安装完成](http://upload-images.jianshu.io/upload_images/9056389-f005018f6abe5393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们输入查询命令后，会发现系统出现三个路径，分别是
```
/usr/sbin/nginx：主程序

/etc/nginx：存放配置文件

/usr/share/nginx：存放静态文件

/var/log/nginx：存放日志        #这个路径可在配置文件中查看到
```
此时我们可以在浏览器中输入ip或者域名，如果出现如下界面，就是安装成功并正在运行了。（安装成功后，会自动运行）

![nginx成功运行](https://upload-images.jianshu.io/upload_images/9056389-cd9b7eb7b15f31ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/12400)


然后我们输入命令，查看nginx默认配置
```
vi  /etc/nginx/nginx.conf     #vi是用自带上古IDE神奇vim打开，一般服务器内都用这个
```
在小白配置nginx教程这篇文章中，我写了后续如何配置nginx里面的内容，大家可以根据自己的需求查看