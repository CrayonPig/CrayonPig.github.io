---
title: element-ui源码之命令
date: 2022.02.15 09:18
tags: [element, vue]
---

上一期给大家分享了element项目的目录结构，这次我们着重分析下package.json 中script每项命令的含义

****

## bootstrap

```
"bootstrap": "yarn || npm i",
```

这个没啥好说的，安装依赖

## build:file

```
"build:file": "node build/bin/iconInit.js & node build/bin/build-entry.js & node build/bin/i18n.js & node build/bin/version.js"
```

这个命令我们可以拆分多个进行逐一分析

#### node build/bin/iconInit.js

```javascript
'use strict';

var postcss = require('postcss');
var fs = require('fs');
var path = require('path');
var fontFile = fs.readFileSync(path.resolve(__dirname, '../../packages/theme-chalk/src/icon.scss'), 'utf8');
var nodes = postcss.parse(fontFile).nodes;
var classList = [];

nodes.forEach((node) => {
  var selector = node.selector || '';
  var reg = new RegExp(/\.el-icon-([^:]+):before/);
  var arr = selector.match(reg);

  if (arr && arr[1]) {
    classList.push(arr[1]);
  }
});

classList.reverse(); // 希望按 css 文件顺序倒序排列

fs.writeFile(path.resolve(__dirname, '../../examples/icon.json'), JSON.stringify(classList), () => {});

```

