---
title: 玩转ubuntu——golong篇
date: 2019.07.16 15:19
tags: [ubuntu, git]
---
```
# 安装go
sudo apt-get remove golang-go
sudo mkdir /soft
cd /soft
sudo wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz 
echo 'export GOPATH=/usr/local/go
export PATH=$PATH:$GOPATH/bin
export NGROK_DOMAIN="potens.top"' | sudo tee -a /etc/profile
source /etc/profile
```
