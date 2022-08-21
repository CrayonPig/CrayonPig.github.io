---
title: Vue源码学习第一期——前期准备
date: 2021.01.14 16:03
tags: [vue, 源码]
---
##小提示
刚开始看源码的时候，一定不要揪着每行代码去看，一定要**不求甚解**，先顺着一条主线去捋清思路，然后再根据自己的理解，分析一些重要的实现。不然就会陷入迷茫，欸，这行干嘛的，啊，这行也不知道。算了，不看了。
举个vue的例子，都知道vue是根据数据状态变化后，产生virtual DOM的方式更新DOM，那么如果你不清楚virtual DOM的实现原理，那就不要去细究virtual DOM的内容。可以先往下梳理思路。

##目录分析
vue源码是托管在github上的，大家可以自己去找。虽然目前vue都更新到3.0了，但2.0目前毕竟是主流，而且大家使用较多，也方便大家去理解具体的思路。所以本次源码采用的是vue2.6.9。
![vue代码目录](https://upload-images.jianshu.io/upload_images/9056389-3f9d4d61b969ca57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
相信很多人跟我一样，第一次看到vue源码目录的时候，都感觉头大。大部分文件夹都不知道是干嘛用的。一般我们会从package.json文件中看起。
package.json主要看scripts 字段和 devDependencies 以及 dependencies 字段，通过 scripts 字段我们可以知道项目中定义的脚本命令。通过 devDependencies 和 dependencies 字段我们可以了解项目的依赖情况。
![package.json](https://upload-images.jianshu.io/upload_images/9056389-20703208514df1de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
vue是采用rollup打包的，rollup是类似于webpack的打包工具，但比webapck更为轻量级，缺点是rollup只对js生效，并不像webpack还可以对图片、css打包。
如果项目有依赖项的话，我们可以运行npm i安装依赖。再根据script的命令去查找运行路径。

当然一般优秀的项目都会有贡献规则文档，里面描述的很清晰。Vue也不例外：[https://github.com/vuejs/vue/blob/dev/.github/CONTRIBUTING.md](https://github.com/vuejs/vue/blob/dev/.github/CONTRIBUTING.md)，在这个文档里说明了一些行为准则，PR指南，Issue Reporting 指南，Development Setup 以及 项目结构。通过阅读这些内容，我们可以了解项目如何开始，如何开发以及目录的说明，下面是对重要目录和文件的简单介绍，这些内容你都可以去自己阅读获取：
```
├── build --------------------------------- 构建相关的文件，一般情况下我们不需要动
├── dist ---------------------------------- 构建后文件的输出目录
├── examples ------------------------------ 存放一些使用Vue开发的应用案例
├── flow ---------------------------------- 类型声明，使用开源项目 [Flow](https://flowtype.org/)
├── package.json -------------------------- 不解释
├── test ---------------------------------- 包含所有测试文件
├── src ----------------------------------- 这个是我们最应该关注的目录，包含了源码
│   ├── runtime--------------------------- 包含了不同的构建或包的入口文件
│   │   ├── web-runtime.js ---------------- 运行时构建的入口，输出 dist/vue.common.js 文件，不包含模板(template)到render函数的编译器，所以不支持 `template` 选项，我们使用vue默认导出的就是这个运行时的版本。大家使用的时候要注意
│   │   ├── web-runtime-with-compiler.js -- 独立构建版本的入口，输出 dist/vue.js，它包含模板(template)到render函数的编译器
│   │   ├── web-compiler.js --------------- vue-template-compiler 包的入口文件
│   │   ├── web-server-renderer.js -------- vue-server-renderer 包的入口文件
│   ├── compiler -------------------------- 编译器代码的存放目录，将 template 编译为 render 函数
│   │   ├── parser ------------------------ 存放将模板字符串转换成元素抽象语法树的代码
│   │   ├── codegen ----------------------- 存放从抽象语法树(AST)生成render函数的代码
│   │   ├── optimizer.js ------------------ 分析静态树，优化vdom渲染
│   ├── core ------------------------------ 存放通用的，平台无关的代码
│   │   ├── observer ---------------------- 反应系统，包含数据观测的核心代码
│   │   ├── vdom -------------------------- 包含虚拟DOM创建(creation)和打补丁(patching)的代码
│   │   ├── instance ---------------------- 包含Vue构造函数设计相关的代码
│   │   ├── global-api -------------------- 包含给Vue构造函数挂载全局方法(静态方法)或属性的代码
│   │   ├── components -------------------- 包含抽象出来的通用组件
│   ├── server ---------------------------- 包含服务端渲染(server-side rendering)的相关代码
│   ├── platforms ------------------------- 包含平台特有的相关代码
│   ├── sfc ------------------------------- 包含单文件组件(.vue文件)的解析逻辑，用于vue-template-compiler包
│   ├── shared ---------------------------- 包含整个代码库通用的代码
```
大概了解了重要目录和文件之后，我们就可以查看 [Development Setup](https://github.com/vuejs/vue/blob/dev/.github/CONTRIBUTING.md#development-setup) 中的常用命令部分，来了解如何开始这个项目了，我们可以看到这样的介绍：

```
# watch and auto re-build dist/vue.js
$ npm run dev

# watch and auto re-run unit tests in Chrome
$ npm run dev:test

```

现在，我们只需要运行 `npm run dev` 即可监测文件变化并自动重新构建输出 dist/vue.js，然后运行 `npm run dev:test` 来测试。不过为了方便，我会在 `examples` 目录新建一个例子，然后引用 dist/vue.js 这样，我们可以直接拿这个例子一边改Vue源码一边看自己写的代码想怎么玩怎么玩。
