---
title: 玩转ubuntu——golong篇
date: 2019.07.17 15:18
tags: [ubuntu, golong]
---
虽然内网穿透是搞定了，但很多小伙伴的需求是微信内开发，微信只能使用80,443端口进行调试，给大家带来了很大的苦恼，所以，本教程新鲜出炉啦
```
upstream ngrok {
server 127.0.0.1:81; # 此处端口要跟 启动服务端ngrok时指定的端口一致
keepalive 64;
}
server {
listen       80;
server_name  domain.com;
#charset koi8-r;
location / {
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Host  $http_host:81; #此处端口要跟 启动服务端ngrok 时指定的端口一致
proxy_set_header X-Nginx-Proxy true;
proxy_pass  http://ngrok;
}
```
