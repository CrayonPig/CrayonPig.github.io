---
title: 玩转ubuntu——内网穿透篇
date: 2019.07.17 14:50
tags: [ubuntu, 内网穿透]
---
很多同学都开发中都有想在手机上调试的需求，又或者是需要在微信上调试，当然我们有很多选择，比如fidder，whistle等工具。也有同学自己购置树莓派，Nas等家用设备，想在外访问家里的设备。
这次为大家介绍内网穿透

百度百科中如此介绍：
>内网穿透，即NAT穿透，网络连接时术语，计算机是局域网内时，外网与内网的计算机节点需要连接通信，有时就会出现不支持内网穿透。就是说映射端口，能让外网的电脑找到处于内网的电脑,提高下载速度。不管是内网穿透还是其他类型的网络穿透，都是网络穿透的统一方法来研究和解决。

通俗点说，让外网电脑能找到内网中的电脑，如此一来，我们就可以做很多有趣的事情。
这次我们将用[ngrok](https://ngrok.com/)进行内网传统

东西准备
- vps服务器（我用的是阿里云的云服务器）
- 属于自己的域名（我用的是阿里云的域名）

ngrok是基于golong编译的，所以我们先安装golong
[玩转ubuntu——golong篇]([https://www.jianshu.com/p/63495013d371](https://www.jianshu.com/p/63495013d371)
)
其次我们需要git进行clone ngrok 的代码，所以安装git
[玩转ubuntu——git篇](https://www.jianshu.com/p/15560c53a601)

```
#clone代码
cd /home
git clone https://github.com/inconshreveable/ngrok.git
cd ./ngrok 
```
我配置自己的域名是mydomain.com，考虑到后续可能有多个设备使用，我建立了一个三级域名*.tunnel.mydomain.com，方便给不同的设备代理
```
#为base域名ddns.hellohl.com生成证书
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=tunnel.mydomain.com" -days 5000 -out rootCA.pem
openssl genrsa -out device.key 2048
openssl req -new -key device.key -subj "/CN=tunnel.mydomain.com" -out device.csr
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000
```
```
#将生成的证书放置在对应的位置
cp rootCA.pem assets/client/tls/ngrokroot.crt
cp device.crt assets/server/tls/snakeoil.crt
cp device.key assets/server/tls/snakeoil.key
```
然后我们需要将生成好的代码编译成我们需要的服务端程序
```
# 默认是生成Linux 64位系统
make release-server 

# Linux 平台 32 位系统
GOOS=linux GOARCH=386 make release-server 
# Linux 平台 64 位系统
GOOS=linux GOARCH=amd64 make release-server 

# Windows 平台 32 位系统
GOOS=windows GOARCH=386 make release-server 
# Windows 平台 64 位系统
GOOS=windows GOARCH=amd64 make release-server 

# MAC 平台 32 位系统
GOOS=darwin GOARCH=386 make release-server 
# MAC 平台 64 位系统
GOOS=darwin GOARCH=amd64 make release-server 

 # ARM 平台
GOOS=linux GOARCH=arm make release-server
```
这里我们需要区分下各自的服务器环境，显示下面的内容则表示编译成功，如果提示失败可能是git或golong版本过低，也可能是网络问题链接不上github，大家可以将所需要的包手动上传至响应目录
```
GOOS="" GOARCH="" go get github.com/jteeuwen/go-bindata/go-bindata
bin/go-bindata -nomemcopy -pkg=assets -tags=release \
	-debug=false \
	-o=src/ngrok/client/assets/assets_release.go \
	assets/client/...
bin/go-bindata -nomemcopy -pkg=assets -tags=release \
	-debug=false \
	-o=src/ngrok/server/assets/assets_release.go \
	assets/server/...
go get -tags 'release' -d -v ngrok/...
github.com/inconshreveable/mousetrap (download)
github.com/rcrowley/go-metrics (download)
Fetching https://gopkg.in/inconshreveable/go-update.v0?go-get=1
Parsing meta tags from https://gopkg.in/inconshreveable/go-update.v0?go-get=1 (status code 200)
get "gopkg.in/inconshreveable/go-update.v0": found meta tag main.metaImport{Prefix:"gopkg.in/inconshreveable/go-update.v0", VCS:"git", RepoRoot:"https://gopkg.in/inconshreveable/go-update.v0"} at https://gopkg.in/inconshreveable/go-update.v0?go-get=1
gopkg.in/inconshreveable/go-update.v0 (download)
github.com/kardianos/osext (download)
github.com/kr/binarydist (download)
Fetching https://gopkg.in/inconshreveable/go-update.v0/check?go-get=1
Parsing meta tags from https://gopkg.in/inconshreveable/go-update.v0/check?go-get=1 (status code 200)
get "gopkg.in/inconshreveable/go-update.v0/check": found meta tag main.metaImport{Prefix:"gopkg.in/inconshreveable/go-update.v0", VCS:"git", RepoRoot:"https://gopkg.in/inconshreveable/go-update.v0"} at https://gopkg.in/inconshreveable/go-update.v0/check?go-get=1
get "gopkg.in/inconshreveable/go-update.v0/check": verifying non-authoritative meta tag
Fetching https://gopkg.in/inconshreveable/go-update.v0?go-get=1
Parsing meta tags from https://gopkg.in/inconshreveable/go-update.v0?go-get=1 (status code 200)
Fetching https://gopkg.in/yaml.v1?go-get=1
Parsing meta tags from https://gopkg.in/yaml.v1?go-get=1 (status code 200)
get "gopkg.in/yaml.v1": found meta tag main.metaImport{Prefix:"gopkg.in/yaml.v1", VCS:"git", RepoRoot:"https://gopkg.in/yaml.v1"} at https://gopkg.in/yaml.v1?go-get=1
gopkg.in/yaml.v1 (download)
github.com/inconshreveable/go-vhost (download)
github.com/alecthomas/log4go (download)
github.com/nsf/termbox-go (download)
github.com/mattn/go-runewidth (download)
github.com/gorilla/websocket (download)
go install -tags 'release' ngrok/main/ngrokd
```
我们可以在./bin/目录中找到文件ngrokd。可以先运行测试一下。
```
#执行ngrokd
./bin/ngrokd -domain="ddns.hellohl.com" -httpAddr=":8080"
```
出现以下提示就说明启动成功
```
[14:28:19 CST 2019/07/17] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [registry] [tun] No affinity cache specified
[14:28:19 CST 2019/07/17] [INFO] (ngrok/log.Info:112) Listening for public http connections on [::]:8080
[14:28:19 CST 2019/07/17] [INFO] (ngrok/log.Info:112) Listening for public https connections on [::]:443
[14:28:19 CST 2019/07/17] [INFO] (ngrok/log.Info:112) Listening for control and proxy connections on [::]:4443
[14:28:19 CST 2019/07/17] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [metrics] Reporting every 30 seconds
```
之后Ctrl+C退出ngrokd，继续来编译ngrok客户端。编译ngrok客户端也很简单，但同样也需要区分响应的环境。
```
# 默认是生成Linux 64位系统
make release-client

# Linux 平台 32 位系统
GOOS=linux GOARCH=386 make release-client
# Linux 平台 64 位系统
GOOS=linux GOARCH=amd64 make release-client

# Windows 平台 32 位系统
GOOS=windows GOARCH=386 make release-client
# Windows 平台 64 位系统
GOOS=windows GOARCH=amd64 make release-client

# MAC 平台 32 位系统
GOOS=darwin GOARCH=386 make release-client
# MAC 平台 64 位系统
GOOS=darwin GOARCH=amd64 make release-client

 # ARM 平台
GOOS=linux GOARCH=arm make release-client
```
编译完成后，在bin/windows_amd64目录下会生成ngrok.exe文件，切记，**ngrokd是服务端程序，ngrok是客户端程序**，把客户端程序下载到本地就可以进行后续操作了

首先执行VPS上的服务器端ngrokd，这里的8080指的是服务器启用8080端口，就是说内网穿透后的域名为xxx.tunnel,mydomain.com:8080。如果在80端口未作他用的情况下，也可将8080端口改为80，这样更方便些。而如果我们VPS的80端口被占用了，但是我们还想用80端口作为服务端口，那么可以使用nginx做一个xxx.tunnel.mydomain.com的反向代理。

```
#服务端执行ngrokd
./bin/ngrokd -domain="tunnel.mydomain.com" -httpAddr=":8080"
```
在本地ngrok.exe所在目录下建立文件ngrok.cfg，用记事本等文本编辑器写入以下内容并保存。
```
server_addr: "ngrok.dingdayu.com:4443"
trust_host_root_certs: false
```
各位小伙伴的webpack一般启动都是8080端口，所以这里以8080端口为例。打开命令提示符，切到ngrok.exe所在目录
```
#启动ngrok客户端
#注意：如果不加参数-subdomain=test，将会随机自动分配子域名。
#
./ngrok -config="ngrok.cfg" -subdomain="test" 8080
```
正常情况下，客户端上会显示以下内容，表示成功连接到服务器端。
```
#客户端ngrok正常执行显示的内容
ngrok                                                  (Ctrl+C to quit)
 
Tunnel Status     online
Version           1.7/1.7
Forwarding        http://ngrok.dingdayu.com:8080 -> 127.0.0.1:8080
Forwarding        https://ngrok.dingdayu.com:8080 -> 127.0.0.1:8080
Web Interface     127.0.0.1:4040
# Conn            0
Avg Conn Time     0.00ms
```
打开浏览器，分别在地址栏中输入http://localhost:8080和http://test.tunnel.mydomain.com:8080，如果后者正常显示并且和http://localhost显示的内容相同，则证明我们已经成功了。

vue-cli以及有些webpack代理都配置好了，页面出现Invalid Host header，这时候我们需要更改下webpack中devServer的配置，大家可根据自己的实际情况进行修改
```
devServer: {
  disableHostCheck: true,
}
```
