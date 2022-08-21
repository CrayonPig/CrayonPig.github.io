---
title: mysql无法登录,更改密码
date: 2019.07.07 14:57
tags: [mysql]
---
![image.png](https://upload-images.jianshu.io/upload_images/9056389-3f88eea7096d2e99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
service mysql stop
mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
mysql -u root mysql
mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';
mysql> FLUSH PRIVILEGES;
mysql> quit
service mysql restart
mysql -uroot -p
Enter password: <输入新设的密码newpassword>
```

参考链接：
[https://blog.csdn.net/ShilohXiang/article/details/89053745](https://blog.csdn.net/ShilohXiang/article/details/89053745)
