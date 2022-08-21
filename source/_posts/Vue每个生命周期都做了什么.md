![Vue2生命周期](https://img.hellohl.com/202206051350689.png)

### beforeCreate

    创建一个空白的 Vue 实例
    
    data method 尚未被初始化，不可使用


​    

### Created

    Vue 实例初始化完成，完成响应式绑定
    
    data method 都已经初始化完成，可调用
    
    尚未开始渲染模板



### beforeMount

    编译模板，调用 render 生成 vdom
    
    还没有开始渲染 dom


​    

### mounted

    完成 dom 渲染
    
    组件创建完成
    
    开始由"创建阶段" 进入 "运行阶段"


​    

### beforeUpdate -- 准备更新但是还没有更新

    data 发生变化之后
    
    准备更新 dom （尚未更新dom）



### updated -- 已经更新完了

    data发生变化，且 dom 更新完毕
    
    （不要在 updated 中修改 data， 可能会导致死循环）



### berforeDestroy(Vue2) 和 beforeUnMount（Vue3） -- 组件销毁之前

    组件进入销毁阶段（尚未销毁，可正常使用）
    
    可移除、解绑一些全局事件、自定义事件



### destroyed(Vue2) 和 unmounted（Vue3）

    组件被销毁了
    
    所有子组件也都被销毁了



### keep-alive 组件的生命周期函数

    onActivated 缓存组件被激活
    
    onDeactivated 缓存组件被隐藏



### Vue 什么时候操作 dom 比较合适

    mounted 和 updated 都不能保证子组件全部挂载完成
    
    使用 $nextTick 渲染 dom



### ajax应该放在哪个生命周期

    created 或者 mounted
    
    推荐：mounted



### vue3 Composition Api 生命周期有何区别？

    用 setup 代替了 beforeCreate 和 created
    
    使用 hooks 函数形式，如 mounted 改为 onMounted()



