---
title: Vue源码运行报错
date: 2021.01.14 16:40
tags: [vue, 源码]
---
我们好不容易从github下载好vue源码，安装好依赖后，运行npm run dev时发现报错


```

> vue@2.6.8 dev D:\workspace\html5\everyday2\vue
> rollup -w -c build/config.js --environment TARGET:web-full-dev

rollup v1.32.1
bundles D:\workspace\html5\everyday2\vue\src\platforms\web\entry-runtime-with-compiler.js → dist\vue.js...
[!] Error: Could not load D:\workspace\html5\everyday2\vue\src\core/config (imported by D:\workspace\html5\everyday2\vue\src\platforms\web\entry-runtime-with-compiler.js): ENOENT: no such file or directory, open 'D:\workspace\html5\everyday2\vue\src\core\config'
Error: Could not load D:\workspace\html5\everyday2\vue\src\core/config (imported by D:\workspace\html5\everyday2\vue\src\platforms\web\entry-runtime-with-compiler.js): ENOENT: no such file or directory, open 'D:\workspace\html5\everyday2\vue\src\core\config'
at load.catch.err (D:\workspace\html5\everyday2\vue\node_modules\rollup\dist\rollup.js:9973:11)
at <anonymous>
at process._tickCallback (internal/process/next_tick.js:182:7)
at Function.Module.runMain (internal/modules/cjs/loader.js:697:11)
at startup (internal/bootstrap/node.js:201:19)
at bootstrapNodeJSCore (internal/bootstrap/node.js:516:3)

```

#### 4、处理报错

其实发生这个错误的原因就是rollup-plugin-alias这个包的问题，windows环境下它将路径的 / 没有换成 \ 的问题

### 方案

**下载 此版本的 rollup-plugin-alias 并进行覆盖原文件。**

**下载地址： https://github.com/ideayuye/rollup-plugin-alias**

**进入目录rollup-plugin-alias，重新编译就好**

执行命令

```
npm install
npm run build
```

重新切回vue目录，执行npm run build，即可运行成功

#### 5、修改package.json

```
"dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev --sourcemap"

```

重新运行后，可看到dist目录下生成了vue.map.js文件。

这样我们在调试源码做测试的时候就可以在浏览器中看到源码了。
