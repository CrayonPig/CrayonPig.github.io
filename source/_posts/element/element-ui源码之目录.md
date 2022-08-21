如果让大家投票选择Vue中最热门的UI框架，我想Element-UI一定是会榜上有名的，接下来就让我们以element-ui v2.15.5版本来学习吧

****
首先从github clone项目

```shell
git clone https://github.com.cnpmjs.org/ElemeFE/element.git
```

项目下载完后，大家可以看到如下目录，主要目录已经标注好了

```
element-v2.15.5
├─.github
├─build（webpack打包启动相关配置）
├─examples（组件使用范例）
├─packages（组件具体实现，css样式放置在这个目录的theme-chalk下）
├─src
|  ├─utils（公共工具函数）
|  ├─transitions（动画配置文件）
|  ├─mixins（组件用的混入文件）
|  ├─locale（语言的配置文件）
|  ├─directives（自定义指令）
|  ├─index.js（组件暴露出口）
├─test（组件测试用例）
├─types（组件ts定义，ts项目引用时会用到）
├─.DS_Store
├─.babelrc
├─.eslintignore
├─.eslintrc
├─.gitattributes
├─.gitignore
├─.travis.yml
├─CHANGELOG.en-US.md
├─CHANGELOG.es.md
├─CHANGELOG.fr-FR.md
├─CHANGELOG.zh-CN.md
├─FAQ.md
├─LICENSE
├─Makefile
├─README.md
├─components.json
├─element_logo.svg
├─package.json
├─yarn-error.log
├─yarn.lock
```





