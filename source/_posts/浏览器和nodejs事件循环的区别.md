---
title: 浏览器和nodejs事件循环的区别
date: 2022.01.15 13:05
---
### 浏览器



#### 单线程和异步

    js是单线程的（无论在浏览器还是 nodejs)
    
    浏览器中 js 执行和 dom 渲染共用一个线程
    
    异步（异步是单线程的解决方案）



#### 异步里面分宏任务和微任务

    宏任务，如setTimeout setInterval ajax请求
    
    微任务，如 promsie async/ await
    
    微任务在下一轮 dom 渲染之前执行，宏任务在下一轮dom渲染之后执行（微任务比宏任务执行的更早一些）



### nodejs异步

    Nodejs同样使用 es 语法，也是单线程，也需要异步
    
    异步任务也分：宏任务 + 微任务
    
    但是，它的宏任务和微任务，分不同类型，有不同的优先级



#### nodejs 宏任务类型和优先级 

    宏任务里面优先级从高到低去执行
    
        Timers - setTimeout setInterval
    
        I/O callbacks - 处理网络、流、TCP 的错误回调
    
        Idle, prepare - 闲置状态（nodejs 内部使用）
    
        Poll 轮询 - 执行 poll 中的 I/O 队列
    
        Check 检查 - 存储 setImmediate 回调
    
        Close callbacks - 关闭回调，如 socket.on('close')
    
    微任务里面优先级从高到低去执行
    
        包括： promsie, async / await, process.nextTick
    
        注意，process.nextTick 优先级最高


​        

#### nodejs event loop

    执行同步代码
    
    执行微任务（process.nextTick 优先级更高）
    
    按顺序执行 6 个类型的宏任务（每当开始之前都执行当前的微任务）



### 总结

浏览器和nodejs的event loop 流程基本相同

nodejs 的宏任务和微任务分类型，按优先级执行