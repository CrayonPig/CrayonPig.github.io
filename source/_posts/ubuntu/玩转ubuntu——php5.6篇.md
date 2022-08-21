---
title: 玩转ubuntu——php5.6篇
date: 2018.04.09 21:34
tags: [ubuntu, php]
---
前情提要：

[玩转ubuntu——nginx篇](https://www.jianshu.com/p/4b2cc653d3cb)

[玩转ubuntu——mysql篇](https://www.jianshu.com/p/a4063b0be975)

Ubuntu 16.04默认安装php7.0环境，php7.0相对于php5.6版本更新较大，如果同学们喜欢用php7.0，那直接看后面nginx怎么配置，如果需要自行安装php5.6需要清除php7的已安装包，否则会报错。

![Ubuntu16.04自带php7.0](https://upload-images.jianshu.io/upload_images/9056389-d7b24c047871bfe0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入命令移出默认及已安装的PHP包
```
sudo dpkg -l | grep php| awk '{print $2}' |tr "\n" " "

sudo apt install aptitude

sudo aptitude purge `dpkg -l | grep php| awk '{print $2}' |tr "\n" " "`

#添加php安装源并更新

sudo add-apt-repository ppa:ondrej/php

sudo apt update
```
我在直接使用sudo add-apt-repository ppa:ondrej/php命令的时候提示报错，command not found情况如下

![添加源报错](https://upload-images.jianshu.io/upload_images/9056389-11a516bc4f102164.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个是缺少程序，安装一下就可以了。输入命令如下：
```
sudo apt-get install software-properties-common python-software-properties  
```
然后再输入增加源的命令，增加的时候需要你enter确认，更新源完成后，我们开始安装mysql5.6，命令如下：
```
$ sudo apt-get install php5.6-fpm php5.6-mysql php5.6-common php5.6-curl php5.6-cli php5.6-mcrypt php5.6-mbstring php5.6-dom
```
安装完成后，我们输入php -v 查看是否安装成功

![php -v](https://upload-images.jianshu.io/upload_images/9056389-64130c967f2a4056.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开php.ini配置文件:
```
sudo vim /etc/php/5.6/fpm/php.ini
```
在esc模式中输入
```
/cgi.fix_pathinfo = 1;
```
查找到cgi.fix_pathinfo = 1;然后把前面分号删除，将其值改为0；也就是
```
cgi.fix_pathinfo = 0;
```
然后重启php5.6-fpm
```
sudo service php5.6-fpm restart
```
打开nginx配置地址，如果忘记怎么查看，可以查看我的上一篇文章，或者输入命令
```
sudo  vi  /etc/nginx/sites-enabled/default
```
如果你是使用的php7.0，那你将nginx配置中

![php7.0自带配置](https://upload-images.jianshu.io/upload_images/9056389-c4c30bac90dce3de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


此代码块注释打开，并在index中增加index.php即可，完整代码如下

![配置好的php7.0](https://upload-images.jianshu.io/upload_images/9056389-1bf937058bd25baf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


配置好后记得重启nginx就可以了。接下来是php5.6的内容

![配置好的php5.6](https://upload-images.jianshu.io/upload_images/9056389-abde826488af4748.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


老规矩，重启nginx
```
sudo service nginx restart
```
然后让我们测试下php是否安装成功
```php
cd /var/www/html

sudo vi phpinfo.php

#在phpinfo.php中输入

<?

php phpinfo();

?>
```
保存即可，打开你的这个文件，我的路径是http://hellohl.com/phpinfo.php，出现如下界面就是成功了


![php安装成功](https://upload-images.jianshu.io/upload_images/9056389-f26375e17a74bb53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
