---
title: cloudstuido-使用问题
date: 2020.05.13 20:48
tags: [H5, cloudstuido, npm]
---
执行cnpm install 后报错
```
[npminstall:runscript:error] chromedriver@^2.27.2 scripts.install run "node install.js" error: RunScriptError: Run "sh -c node install.js" error, exit code null
✖ Install fail! RunScriptError: post install error, please remove node_modules before retry!
Run "sh -c node install.js" error, exit code null
RunScriptError: post install error, please remove node_modules before retry!
Run "sh -c node install.js" error, exit code null
    at ChildProcess.proc.on.code (/usr/lib/node_modules/cnpm/node_modules/runscript/index.js:96:21)
    at ChildProcess.emit (events.js:198:13)
    at maybeClose (internal/child_process.js:982:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:259:5)
npminstall version: 3.27.0
npminstall args: /usr/bin/node /usr/lib/node_modules/cnpm/node_modules/npminstall/bin/install.js --fix-bug-versions --china --userconfig=/root/.cnpmrc --disturl=https://npm.taobao.org/mirrors/node --registry=https://r.npm.taobao.org
```
![image.png](https://upload-images.jianshu.io/upload_images/9056389-104bb31fff799024.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

查了好多方法都没用，后处理如下
1.如果执行过npm install，先删除 node_modules 文件夹，不然运行的时候可能会报错
2.执行下面的命令
```
npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
```
3.再执行 npm install 即可正常下载

问题原因
经分析发现，某些版本下，chromedriver 的 zip 文件 url 的响应是 302 跳转，而在 install.js 里使用的是 Node.js 内置的 http 对象的 get 方法无法处理 302 跳转的情况；而在另外一些情况下，则是因为 googleapis.com 被墙了，此时即使采用科学 上网的方法也仍然无法获取文件。
