---
title: 玩转ubuntu——mysql篇
date: 2018.04.09 17:37
tags: [ubuntu, mysql]
---
前情提要：

[玩转ubuntu——nginx篇](https://www.jianshu.com/p/4b2cc653d3cb)

上次讲了如何配置ubuntu16.04中的nginx，这次接着讲，如何配置mysql，首先输入命令

```nginx
sudo apt update    #更新源

sudo apt install mysql-server    #安装mysql相关服务
```

老规矩，y继续安装，然后可见下图

![设置数据库密码](https://upload-images.jianshu.io/upload_images/9056389-9e09147829782074.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![确认密码](https://upload-images.jianshu.io/upload_images/9056389-9ea78f5d734a7b45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>数据库密码一定要记住，不然忘记后修改数据库密码还是挺麻烦的

为了方便大家使用数据库，我介绍一款软件，navicat，它长这样

![mysql可视化软件](https://upload-images.jianshu.io/upload_images/9056389-856b5515398bf1fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


网上现在破解的不好找了，等过段时间服务器搭建好后，我把下载链接放上来。

打开navicat后，我们可以看到如下界面
![点击连接](https://upload-images.jianshu.io/upload_images/9056389-93645aaef06a48aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![连接](https://upload-images.jianshu.io/upload_images/9056389-db6e9eeda62858c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![ssh连接](https://upload-images.jianshu.io/upload_images/9056389-791cd5cff7f77791.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



很多同学连接后会报错，报错如下：
![mysql连接报错](https://upload-images.jianshu.io/upload_images/9056389-036ae5229f192585.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其实这个错误是因为你没有给当前用户访问数据库的权限造成的，解决方案如下：

进入服务器，输入代码
```
sudo vi /etc/ssh/sshd_config
```
在文件最下面添加代码（快捷键为shift+g）
```
KexAlgorithms diffie-hellman-group1-sha1,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha1Ciphers 3des-cbc,blowfish-cbc,aes128-cbc,aes128-ctr,aes256-ctr
```
然后再执行如下命令
```
ssh-keygen -A  

service ssh restart         #重启SSH
```
然后再连接数据库，就会发现连接成功

![mysql连接成功](https://upload-images.jianshu.io/upload_images/9056389-057a20f2bf5d134c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


新安装的mysql下面是有四个实例的
![navicat](https://upload-images.jianshu.io/upload_images/9056389-51407925c82f15ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



新建mysql自带实例
information_schema：是MySQL系统自带的数据库，它提供了数据库元数据的访问方式。说白了就是mysql的各种操作都是因为把mysql的方法存在这个实例中了

mysql：这个是mysql的核心数据库，类似于sql server中的master表，主要负责存储数据库的用户、权限设置、关键字等mysql自己需要使用的控制和管理信息。不可以删除，如果对mysql不是很了解，也不要轻易修改这个数据库里面的表信息。

PERFORMANCE_SCHEMA：5.5开始新增的一个数据库，主要用于收集数据库服务器性能参数。并且库里表的存储引擎均为PERFORMANCE_SCHEMA，而用户是不能创建存储引擎为PERFORMANCE_SCHEMA的表。

sys是一个MySQL自带的系统库，在安装MySQL 5.7以后的版本，使用mysqld进行初始化时，会自动创建sys库，sys库里面的表、视图、函数、存储过程可以使我们更方便、快捷的了解到MySQL的一些信息。

作为小白，我们当然不需要考虑这四个实例究竟怎么用，我们只需要知道，这四个实例不能修改不能删除就可以了。