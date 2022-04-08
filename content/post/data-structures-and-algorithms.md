---
title: "笔记|数据结构与算法"
date: 2021-12-05
draft: true
categories: ["数据结构与算法"]
tags: ["学习笔记", "数据结构", "算法"]
---

# Table of Contents

1.  [基础概念](#org80887fd)
2.  [时间、空间复杂度分析](#orgba8bf63)
3.  [数组](#org6e34a1c)
4.  [链表](#org57850f1)



<a id="org80887fd"></a>

# 基础概念

-   数据结构：数据的存储结构
-   算法：操作数据的方法
-   数据结构为算法服务，算法作用在特定的数据结构之上


<a id="orgba8bf63"></a>

# 时间、空间复杂度分析

-   在计算机运行一遍，统计效率：事后统计法：缺点是有环境依赖，受数据规模影响。
-   时间复杂度分析
    -   渐进时间复杂度，表示算法的执行时间与数据规模之间的增长关系
    -   只关注循环执行次数最多的一段代码
    -   加法法则：总复杂度等于量级最大的那段代码的复杂度
    -   乘法法则：嵌套代码的复杂度等于嵌套内外代码复杂度的乘积
-   复杂度量级
    -   多项式量级
        -   常量阶 O(1)
        -   对数阶 O(logn)
        -   线性阶 O(n)
        -   线性对数阶 O(nlogn)
        -   平方阶 O(n<sup>2</sup>), 立方阶 O(n<sup>3</sup>) &#x2026; k次方阶 O(n<sup>k</sup>)
        -   O(m+n), O(m\*n): m和n是两个数据的规模
    -   非多项式量级
        -   指数阶 O(2<sup>n</sup>)
        -   阶乘阶 O(n!)
-   空间复杂度分析
    -   渐进空间复杂度，表示算法的存储空间与数据规模之间的增长关系
    -   常见空间复杂度：O(1), O(n), O(n<sup>2</sup>)
-   最好情况时间复杂度
-   最坏情况时间复杂度
-   平均情况时间复杂度：数学期望，每一种情况乘以对应的概率
-   均摊时间复杂度：对一个数据结构进行一组连续操作中，大部分情况下时间复杂度都很低，只有个别情况下时间复杂度比较高，而且这些操作之间存在前后连贯的时序关系，这个时候，我们就可以将这一组操作放在一块儿分析，看是否能将较高时间复杂度那次操作的耗时，平摊到其他那些时间复杂度比较低的操作上。


<a id="org6e34a1c"></a>

# 数组

-   概念：数组是一种线性表数据结构。它用一组连续的内存空间 ，来存储一组具有相同类型的数据。
-   优势：可以时间随机访问，根据下标随机访问的时间复杂度为 O(1)。
-   劣势：插入和删除操作低效，插入和删除的平均时间复杂度为 O(n)。
    
    -   优化插入：借助快排思想
        
        ![img](/Users/geekinney/Pictures/blog/enhance-array-insert.jpg)
    -   优化删除：JVM标记清除垃圾回收算法的核心思想 (如何理解???)
    
    为了避免 d，e，f，g，h 这几个数据会被搬移三次，我们可以先记录下已经删除的数据。每次的删除操作并不是真正地搬移数据，只是记录数据已经被删除。当数组没有更多空间存储数据时，我们再触发执行一次真正的删除操作，这样就大大减少了删除操作导致的数据搬移。
    
    ![img](/Users/geekinney/Pictures/blog/enhance-array-delete.jpg)
-   数组访问越界问题：C中数组越界是未决行为，没有规定数组访问越界时编译器如何处理。越界的数组会访问到其他的内存，因此会出现逻辑错误及被计算机病毒利用。
-   容器与数组
    -   数组的容器类：Java 的 ArrayList, C++ 的 vector
    -   容器将数组的操作细节封装起来，支持动态扩容
    -   数组扩容时涉及的内存申请及数据迁移比较耗时，因此创建 ArrayList 需事先指定合适的容量大小。
    -   容器类的缺点
        -   无法存储基本类型：int, long 需要封装为 Integer, Long 类，开箱和装箱的过程有性能消耗。因此关注性能或希望使用基本类型，应使用数组。
        -   数据大小已知，并且对数据的操作简单，无需使用 ArrayList。
        -   表示多维数组时，使用数组更加直观:
            -   Object[][] array
            -   ArrayList<ArrayList<object>> array
        -   做底层开发时，如网络框架，性能需要优化到极致，选择数组。
-   为什么数组从0开始编号，而不是1？
    -   下标的确切含义是“偏移”。用 a 表示数组的首地址，a[0]就是偏移为0的地址。a[k]表示偏移k个 type<sub>size</sub> 的地址。
    -   从0开始的计算公式为：a[k]<sub>address</sub> = base<sub>address</sub> + k \* type<sub>size</sub>
    -   从1开始的计算公式为：a[k]<sub>address</sub> = base<sub>address</sub> + (k-1) \* type<sub>size</sub>
    -   因此 0 比 1 少了一次减法运算，效率优先 以及 C 的历史原因


<a id="org57850f1"></a>

# 链表

-   应用场景：LRU缓存淘汰算法(CPU缓存，数据库缓存，浏览器缓存..)
    -   先进先出策略(FIFO)
    -   最少使用策略(LFU)
    -   最近最少使用策略(LRU)
-   存储结构
    -   数组：数组需要 **一块连续的内存空间** 来存储，对内存的要求比较高。如果我们申请一个 100MB 大小的数组，当内存中没有连续的、足够大的存储空间时，即便内存的剩余总可用空间大于 100MB，仍然会申请失败。
    -   链表：链表并不需要一块连续的内存空间，它通过“指针”将一组 **零散的内存块串联起来** 使用，所以如果我们申请的是 100MB 大小的链表，根本不会有问题。
-   单链表
    -   每个结点有一个后继指针指向后面的结点
    -   尾结点指针指向空地址
    -   插入和删除操作的时间复杂度为 O(1)
    -   随机访问的平均时间复杂度是 O(n)
-   循环链表
    -   尾结点指针指向头结点
    -   从链尾到链头很方便
    -   约瑟夫问题
-   双向链表
    -   每个结点有一个后继指针指向后面的结点 和 一个前驱指针指向前面的结点
    -   占用更多的内存空间，但支持双向遍历
        
        ![img](/Users/geekinney/Pictures/blog/double-link-list.jpg)
    -   插入、删除操作比单链表更高效
    -   Java 的 LinkedHashMap 用到了双向链表
    -   执行较慢的程序，通过消耗更多的内存来优化（空间换时间）
    -   消耗过多内存的程序，通过消耗更多的时间来降低内存消耗（时间换空间）
-   双向循环链表
-   数组、链表对比
    -   数组简单易用，在实现上使用的是连续的内存空间，可以借助 CPU 的缓存机制，预读数组中的数据，所以访问效率更高。而链表在内存中并不是连续存储，所以对 CPU 缓存不友好，没办法有效预读。
    -   数组的缺点是大小固定，一经声明就要占用整块连续内存空间。如果声明的数组过大，系统可能没有足够的连续内存空间分配给它，导致“内存不足（out of memory）”。如果声明的数组过小，则可能出现不够用的情况。这时只能再申请一个更大的内存空间，把原数组拷贝进去，非常费时。链表本身没有大小的限制， **天然地支持动态扩容** ，我觉得这也是它与数组最大的区别。
    -   如果你的代码对内存的使用非常苛刻，那数组就更适合你。因为链表中的每个结点都需要消耗额外的存储空间去存储一份指向下一个结点的指针，所以内存消耗会翻倍。而且，对链表进行频繁的插入、删除操作，还会导致频繁的内存申请和释放，容易造成内存碎片，如果是 Java 语言，就有可能会导致频繁的 GC（Garbage Collection，垃圾回收）。
-   LRU 缓存实现
    -   我的思路是这样的：我们维护一个有序单链表，越靠近链表尾部的结点是越早之前访问的。当有一个新的数据被访问时，我们从链表头开始顺序遍历链表。
    -   如果此数据之前已经被缓存在链表中了，我们遍历得到这个数据对应的结点，并将其从原来的位置删除，然后再插入到链表的头部。
    -   如果此数据没有在缓存链表中，又可以分为两种情况：如果此时缓存未满，则将此结点直接插入到链表的头部；如果此时缓存已满，则链表尾结点删除，将新的数据结点插入链表的头部。
    -   可以引入 散列表 优化该算法
-   问题思考：如何字符串通过单链表存储，如何判断回文字符串?
-   写链表代码注意点
    -   理解指针or引用：存储所指对象的内存地址。将某个变量赋值给指针，实际上就是将这个变量的地址赋值给指针；或者反过来说，指针中存储了这个变量的内存地址，指向了这个变量，通过指针就能找到这个变量。
    -   警惕指针丢失和内存泄漏：链表插入结点时的顺序问题
    -   利用哨兵简化时间难度：用于解决边界问题，不直接参与业务逻辑。
        
        -   引入哨兵结点，无论链表是否为空，head指针都指向一个哨兵结点。
        -   有哨兵结点的链表称为带头链表，没有的称为无头链表。(查找数组中特定值序号的问题)
        
        ![img](/Users/geekinney/Pictures/blog/linklist-with-head.jpg)
    -   重点留意边界条件处理：考虑各种特殊情况下能否正常处理。
    -   举例画图，辅助思考
    -   多写多练，没有捷径
        -   单链表反转
        -   链表中环的检测
        -   两个有序的链表合并
        -   删除链表倒数第 n 个结点
        -   求链表的中间结点