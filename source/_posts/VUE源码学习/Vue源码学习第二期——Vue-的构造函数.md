---
title: Vue源码学习第二期——Vue-的构造函数
date: 2021.01.15 14:58
tags: [vue, 源码]
---
承上文，我们知道，我们可以使用npm run dev命令来运行项目。
```
"dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev",
```
其中 -w 就是watch，-c 就是指定配置文件为 build/config.js，简单来说dev命令是rollup使用scripts/config.js的配置运行，并设置了一个变量TARGET的值为web-full-dev。
那么我们来看scripts/config.js中的代码
```
if (process.env.TARGET) {
  module.exports = genConfig(process.env.TARGET)
} else {
  exports.getBuild = genConfig
  exports.getAllBuilds = () => Object.keys(builds).map(genConfig)
}
```
如果有TARGET，那么将TARGET的值作为参数传入genConfig方法，我们继续看genConfig方法
```
function genConfig (name) {
  const opts = builds[name]
  const config = {
    input: opts.entry,
    external: opts.external,
    plugins: [
      flow(),
      alias(Object.assign({}, aliases, opts.alias))
    ].concat(opts.plugins || []),
    output: {
      file: opts.dest,
      format: opts.format,
      banner: opts.banner,
      name: opts.moduleName || 'Vue'
    },
    onwarn: (msg, warn) => {
      if (!/Circular/.test(msg)) {
        warn(msg)
      }
    }
  }
 // built-in vars
  const vars = {
    __WEEX__: !!opts.weex,
    __WEEX_VERSION__: weexVersion,
    __VERSION__: version
  }
  // feature flags
  Object.keys(featureFlags).forEach(key => {
    vars[`process.env.${key}`] = featureFlags[key]
  })
  // build-specific env
  if (opts.env) {
    vars['process.env.NODE_ENV'] = JSON.stringify(opts.env)
  }
  config.plugins.push(replace(vars))

  if (opts.transpile !== false) {
    config.plugins.push(buble())
  }

  Object.defineProperty(config, '_name', {
    enumerable: false,
    value: name
  })

  return config
}
```
可以看到，在genconfig中，使用传来的TARGET值（web-full-dev）从builds中获取参数，然后再定义一个相关的config参数。
从builds的定义中，我们可以知道获取到的数据为
```
'web-full-dev': {
    entry: resolve('web/entry-runtime-with-compiler.js'),
    dest: resolve('dist/vue.js'),
    format: 'umd',
    env: 'development',
    alias: { he: './entity-decoder' },
    banner
  }
```
最终通过gencofig运行结果，我们可以知道，入口文件是
```
src/platforms/web/web-runtime-with-compiler.js
```
那么我们继续找这个文件，一进去，我们就发现我们要找到的东西了
```
import Vue from './runtime/index'
```
继续按照目录查找，这个文件还是一样的
```
import Vue from 'core/index'
```
依照此思路，最终我们寻找到Vue构造函数的位置应该是在 src/core/instance/index.js 文件中，其实我们看上节的目录分析，应该也可以知道，包含Vue构造函数设计相关的代码就是放在instance中。
那好，接下来，我们看正题
```
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue
```
引入依赖，定义 Vue 构造函数，然后以Vue构造函数为参数，调用了五个方法，最后导出 Vue。这五个方法分别来自五个文件：init.js，state.js， render.js，events.js 以及 lifecycle.js
分别进入这五个文件，你会发现这都是在 Vue 的原型 prototype 上挂载方法或属性。

**那么问题来了，为什么Vue使用的是function创建的构造函数而不是Class呢，原因是Vue 按功能把这些扩展分散到多个模块中去实现，⽽不是在⼀个模块⾥实现所有，这种⽅式是⽤ Class 难以实现的。这么做的好处是⾮常⽅便代码的维护和管理**


方法挂载完成后是这样的：
```
// initMixin(Vue)    src/core/instance/init.js **************************************************
Vue.prototype._init = function (options?: Object) {}

// stateMixin(Vue)    src/core/instance/state.js **************************************************
Vue.prototype.$data
Vue.prototype.$set = set
Vue.prototype.$delete = del
Vue.prototype.$watch = function(){}

// renderMixin(Vue)    src/core/instance/render.js **************************************************
Vue.prototype.$nextTick = function (fn: Function) {}
Vue.prototype._render = function (): VNode {}
Vue.prototype._s = _toString
Vue.prototype._v = createTextVNode
Vue.prototype._n = toNumber
Vue.prototype._e = createEmptyVNode
Vue.prototype._q = looseEqual
Vue.prototype._i = looseIndexOf
Vue.prototype._m = function(){}
Vue.prototype._o = function(){}
Vue.prototype._f = function resolveFilter (id) {}
Vue.prototype._l = function(){}
Vue.prototype._t = function(){}
Vue.prototype._b = function(){}
Vue.prototype._k = function(){}

// eventsMixin(Vue)    src/core/instance/events.js **************************************************
Vue.prototype.$on = function (event: string, fn: Function): Component {}
Vue.prototype.$once = function (event: string, fn: Function): Component {}
Vue.prototype.$off = function (event?: string, fn?: Function): Component {}
Vue.prototype.$emit = function (event: string): Component {}

// lifecycleMixin(Vue)    src/core/instance/lifecycle.js **************************************************
Vue.prototype._mount = function(){}
Vue.prototype._update = function (vnode: VNode, hydrating?: boolean) {}
Vue.prototype._updateFromParent = function(){}
Vue.prototype.$forceUpdate = function () {}
Vue.prototype.$destroy = function () {}
```
然后顺着我们刚来的路，再走回去，首先是‘src/core/index’，
```
import Vue from './instance/index'
import { initGlobalAPI } from './global-api/index'
import { isServerRendering } from 'core/util/env'
import { FunctionalRenderContext } from 'core/vdom/create-functional-component'

initGlobalAPI(Vue)

Object.defineProperty(Vue.prototype, '$isServer', {
  get: isServerRendering
})

Object.defineProperty(Vue.prototype, '$ssrContext', {
  get () {
    /* istanbul ignore next */
    return this.$vnode && this.$vnode.ssrContext
  }
})

// expose FunctionalRenderContext for ssr runtime helper installation
Object.defineProperty(Vue, 'FunctionalRenderContext', {
  value: FunctionalRenderContext
})

Vue.version = '__VERSION__'

export default Vue

```
这个文件较为简单，在Vue挂载了globalAPI，在Vue原型prototype挂载了$isServer和$ssrContext，在 Vue 上挂载了 version 属性，监听FunctionalRenderContext。
经过initGlobalAPI挂载后，此时Vue的样子是：
```
// src/core/index.js / src/core/global-api/index.js
Vue.config
Vue.util = util
Vue.set = set
Vue.delete = del
Vue.nextTick = util.nextTick
Vue.options = {
    components: {
        KeepAlive
    },
    directives: {},
    filters: {},
    _base: Vue
}
Vue.use
Vue.mixin
Vue.cid = 0
Vue.extend
Vue.component = function(){}
Vue.directive = function(){}
Vue.filter = function(){}

Vue.prototype.$isServer
Vue.prototype.$ssrContext
Vue.version = '__VERSION__'
```
Vue.options可能稍微难理解一些，我们后续再进行分析。下一个就是 src/platforms/runtime/index.js 文件了，主要做了三件事儿

1、覆盖 Vue.config 的属性，将其设置为平台特有的一些方法
2、Vue.options.directives 和 Vue.options.components 安装平台特有的指令和组件
3、在 Vue.prototype 上定义 __patch__ 和 $mount

这三件事情完成之后，Vue 变成下面这个样子：

```
// 安装平台特定的utils
Vue.config.isUnknownElement = isUnknownElement
Vue.config.isReservedTag = isReservedTag
Vue.config.getTagNamespace = getTagNamespace
Vue.config.mustUseProp = mustUseProp
// 安装平台特定的 指令 和 组件
Vue.options = {
    components: {
        KeepAlive,
        Transition,
        TransitionGroup
    },
    directives: {
        model,
        show
    },
    filters: {},
    _base: Vue
}
Vue.prototype.__patch__
Vue.prototype.$mount
```
这里大家要注意的是 Vue.options 的变化。另外这里的 $mount 方法很简单：
```
Vue.prototype.$mount = function (
  el?: string | Element,
  hydrating?: boolean
): Component {
  el = el && inBrowser ? query(el) : undefined
  return this._mount(el, hydrating)
}
```
首先根据是否是浏览器环境决定要不要 query(el) 获取元素，然后将 el 作为参数传递给 this._mount()。

最后一个处理 Vue 的文件就是入口文件 web-runtime-with-compiler.js 了，该文件做了两件事：

1、缓存来自 web-runtime.js 文件的 $mount 函数
```
const mount = Vue.prototype.$mount
```
然后覆盖覆盖了 Vue.prototype.$mount

2、在 Vue 上挂载 compile
```
Vue.compile = compileToFunctions
```
compileToFunctions 函数的作用，就是将模板 template 编译为render函数。

至此，我们算是还原了 Vue 构造函数，总结一下：

```
1、Vue.prototype 下的属性和方法的挂载主要是在 src/core/instance 目录中的代码处理的

2、Vue 下的静态属性和方法的挂载主要是在 src/core/global-api 目录下的代码处理的

3、runtime/index.js 主要是添加web平台特有的配置、组件和指令，web-runtime-with-compiler.js 给Vue的 $mount 方法添加 compiler 编译器，支持 template。
```
