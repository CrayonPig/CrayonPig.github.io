介绍 diff 算法

    diff算法很早就有
    
    diff算法应用广泛
    
    如果要严格 diff 两棵树，事件复杂度 O (n ^ 3)，不可用了



Tree diff 的优化

    只比较同一级，不夸级比较
    
    tag 不同则删掉重建（不再去比较内部的细节）
    
    子节点通过key区分



React diff  - 仅右移



![img](https://img.hellohl.com/202206051416892.jpeg)

Vue2 双端比较

![image-20220605141752554](https://img.hellohl.com/202206051417601.png)

Vue3 最长递增子序列



![image-20220605141818892](https://img.hellohl.com/202206051418961.png)