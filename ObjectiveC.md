# Objective-C

## 1. 内存管理方案

### Tagged Pointer
#### 结构
对于NSNumer、NSData，如果其中存放的是比较小的对象，则指针的值不再是地址，而是真正的值。它的内存不在堆中，不需要malloc和free。
#### 结论
苹果将Tagged Pointer引入，给64位系统带来了内存的节省和运行效率的提高。Tagged Pointer通过在其最后一个 bit 位设置一个特殊标记，用于将数据直接保存在指针本身中。因为Tagged Pointer并不是真正的对象。

### NONPOINTER_ISA
#### 结构
isa 的联合体中，在第一位存储了 nonpointer。表示是否对isa开启指针优化 。0代表是纯isa指针，1代表除了地址外，还包含了类的一些信息、对象的引用计数等


### 散列表-SideTable
#### 结构
```c
struct SideTable {
    spinlock_t slock;
    RefcountMap refcnts; 
    weak_table_t weak_table;
}
```
一个程序中有一个 SideTables()，包含若干个SideTable。在arm64中是8个。
一个散列表包含自旋锁、引用计数表、弱引用表。
SideTables、引用计数表、若引用表都是Hash表。
#### 应用场景
通过一个对象指针，来找到它的引用计数表或者弱引用表具体在哪个sideTable中。
### 参考链接
https://blog.csdn.net/weixin_30682415/article/details/100094332
https://www.jianshu.com/p/1637133949d8
https://juejin.cn/post/6861741320701607950
https://juejin.cn/post/7047469212658302990
https://www.jianshu.com/p/fe2df7d931ed
https://www.jianshu.com/p/c38013a233bf
https://blog.csdn.net/LinShunIos/article/details/120195389
