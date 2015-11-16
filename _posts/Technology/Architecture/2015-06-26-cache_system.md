---

layout: post
title: 缓存系统
category: 技术
tags: Architecture
keywords: 缓存 redis

---

## 简介 

第一次接触缓存，是在学习hibernate的时候，hibernate可以将查询结果缓存起来，加快下次查询的速度。第一次自己写缓存（其实就是个map），是因为被告知某个查询操作，批量查询的效率更高，这个map用来暂存批量查询的处理结果。但要说将缓存系统作为应用服务器和数据库服务器的中间层（缓存系统作为单独的一层，并上升成为一个独立的组件），还是在学习redis的时候。

在计算机和网络领域，缓存无处不在。可以这么说，只要硬件性能不对等的地方都会有缓存的身影。

## 缓存系统

使用缓存系统，最理想的效果是：应用系统尽量只与缓存系统交互，只有在查询缓存失败时，才访问数据库。进而，将读写压力从数据库转移到缓存系统上。

缓存系统有以下几类：

1. 作为一个组件存在（或者说，本地缓存。比如一个jar提供的java类）
2. 单机的、独立的应用
3. 跨主机的、独立的应用

一个缓存系统应该考虑如下特性：

1. 是否可以线性扩展，即通过增加主机，来增加缓存系统的存储能力，这涉及到分布式缓存系统。

    一旦涉及到分布式缓存系统，那么涉及到
    
       - 如何将缓存的数据均摊到所有缓存节点
       - 如果某个节点失效，如何处理

2. 线程安全，在线程操作时，维护数据的一致性
3. 当实际数据发生改变时，如何及时感知并更新缓存
4. 如果缓存系统容量一定，当添加新的数据时，没有剩余空间，如何处理？数据是否有有效期？
4. 最重要的一点，不能太复杂，如果访问延迟稍高，缓存系统便失去了存在的意义。

## 缓存系统与数据库的一致性（未完待续）

1. 数据加入缓存

    - 客户端查询缓存，如果缓存中没有，则查询数据库，并将查询结果加入到缓存中。

2. 数据从缓存清除或更新

    - 数据库中数据全部加入缓存：定期将缓存中数据与数据库同步
    - 数据库中数据部分加入缓存：在向数据库写入数据的同时，告诉缓存该数据应失效

## 缓存系统的数据模型（未完待续）

### key value模型

数据对象转为json字符串作为value
    
## 引用

[应用系统数据缓存设计][]

[Guava学习笔记：Guava cache][]

[面向GC的Java编程][]

[Apache Commons Pool的入门例子][]

[应用系统数据缓存设计]: http://www.tuicool.com/articles/nYvy2a
[Guava学习笔记：Guava cache]: http://www.cnblogs.com/peida/p/Guava_Cache.html
[面向GC的Java编程]: http://coolshell.cn/articles/11541.html
[Apache Commons Pool的入门例子]: http://blog.csdn.net/fwing/article/details/5525124
[每天进步一点点——五分钟理解一致性哈希算法(consistent hashing)]: http://blog.csdn.net/cywosp/article/details/23397179