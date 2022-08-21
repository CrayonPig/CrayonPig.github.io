---
title: 小白配置nginx教程
date: 2018.04.09 17:03
tags: [ubuntu, nginx]
---
很多朋友都疑惑nginx到底应该怎么配置，我以ubuntu16.04最新安装nginx后来说明。
```nginx
user www-data; #运行用户，一般不做修改

worker_processes auto;

#nginx进程，一般设置为cpu数量或者cpu数量的两倍

pid /run/nginx.pid;

#pid（进程标识符）存放路径

events {

  worker_connections 768;

  #每个工作进程的最大连接数量，根据硬件调整，小白保持默认

  # multi_accept on;

  #multi_accept在Nginx接到一个新连接通知后调用accept()来接受尽量多的连接，一般不用

}

http {

  #设定http服务器，利用它的反向代理功能提供负载均衡支持

  ##

  # Basic Settings

  ##基本设置

  sendfile on;

  #开启高效传输模式

  tcp_nopush on;

  #激活tcp_nodelay，内核会等待将更多的字节组成一个数据包，从而提高I/O性能

  tcp_nodelay on;

  keepalive_timeout 65;

  #连接超时时间，单位是秒

  types_hash_max_size 2048;

  # server_tokens off;

  # 隐藏响应header和错误通知中的版本号

  # server_names_hash_bucket_size 64;

  # server_name_in_redirect off;

  # 设定请求缓存

  include /etc/nginx/mime.types;

  # 文件扩展名与类型映射表

  default_type application/oc tet-stream;

  # 默认文件类型

  ##

  # SSL Settings

  ## ssl设置

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;# Dropping SSLv3, ref: POODLE

  # 允许SSL协议

  ssl_prefer_server_ciphers on;

  # 启动加密算法

  ##

  # Logging Settings

  ##

  access_log /var/log/nginx/access.log;

  # 日志格式及日志存放路径

  error_log /var/log/nginx/error.log;

  # 错误日志存放路径

  ##

  # Gzip Settings

  ## gzip on;

  # 开启gzip压缩功能

  gzip_disable "msie6";

  # gzip_vary on;

  # gzip_proxied any;

  # gzip_comp_level 6;

  # gzip_buffers 16 8k;

  # gzip_http_version 1.1;

  # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

  ##

  # Virtual Host Configs

  ##

  include /etc/nginx/conf.d/*.conf;

  include /etc/nginx/sites-enabled/*;

  # 引用文件中的配置

}

#mail { # 邮件代理服务器配置

  # # See sample authentication script at:

  # # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript # # # auth_http localhost/auth.php;

  # # pop3_capabilities "TOP" "USER";

  # # imap_capabilities "IMAP4rev1" "UIDPLUS";

  # # server {

  # listen localhost:110;

  # protocol pop3;

  # proxy on;

# }

# server{

  # listen localhost:143;

  # protocol imap;

  # proxy on;

# }

#}
```
当然我们作为小白是不需要知道这些代码的全部使用方法的，我们只需要知道我们需要改哪里就行，我们配置服务器一般就是配置路径什么的，所以我们只需要看http代码块内的东西，粗略的看下，我们可以发现，很多代码都被注释掉了，我们只能进include中引用的配置看。服务器中输入命令
```
cd  /etc/nginx/conf.d/ #进入这个路径

ls   #罗列该路径下的文件
```
命令输入后，我发现，该路径下并没有文件，说明不是这个引用路径中的配置生效的，那么我们再看下一个引用路径，输入命令
```
cd   /etc/nginx/sites-enabled/

ls  
```

![ls命令后](https://upload-images.jianshu.io/upload_images/9056389-bcfb135f3ebbfa79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们可以发现，这个路径下是有文件的，那么我们用vim打开它看看他里面写了什么
```
vi  default
```

![default文件](https://upload-images.jianshu.io/upload_images/9056389-452419c2b424d6f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们可以看到大部分代码都是注释的，我们不看注释的内容，只看生效的代码，发现他是一个server代码块，内容如下：
```
server {    

    listen 80 default_server;       

    listen [::]:80 default_server;   

    #监听80端口，第一个是ipv4，第二个是ipv6    

    root /var/www/html;   

    #网站路径   

     index index.html index.htm index.nginx-debian.html;    

    #主页    server_name _;   

    # 配置基于名称的虚拟主机，一般用于一台服务器分配多个域名   

     location / {       

         try_files $uri $uri/ =404;       

        #如果找不到文件，返回404，也可以自己指定文件   

     }

}
```
那我们修改相关路径和配置就可以配置自己的服务器了。vim修改命令是需要先敲键盘i，进入编辑模式，然后修改好后，使用esc键退出编辑模式，用：号输入wq保存，或者q!不保存强制退出，然后重启nginx，输入命令
```
sudo service nginx restart
```
重启成功后，就可以看到你的配置生效了，重启失败说明你刚修改的格式神马的有问题，细心检查就好