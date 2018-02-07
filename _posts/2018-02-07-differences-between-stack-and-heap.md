---
layout: default
title:  "differents between stack and heap"
date:   2018-02-07 20:47:22 +0800
categories: "memory management"
---

# 堆栈区别
相同点：都是抽象的概念，计算机具体表示为数据结构。运行时分配实际内存，都在RAM上。  

# 不同点

## 堆
* 需要自己手动管理内存申请和释放：通过这些方法申请的内存都在堆上`malloc, realloc, calloc,and free`.
* 堆在应用（进程）启动的时候被分配。当应用（进程）退出的时候，堆内存被回收。
* 堆的大小在应用（进程） 启动的时候被设置，但是可以动态增长。
* 堆的存取速度会比栈慢，对是全局资源，通常必须是多线程安全，即每个分配和释放需要同步。

## 栈
* 不需要自己手动管理内存释放.
* 栈跟线程绑定在一起,每个线程一般都会有一个栈,随着线程的执行完毕栈内存被系统回收。
* 栈的大小在线程创建的时候被设置。影响他大小的因素有很多，包括，程序语言，机器架构，多线程，系统剩余可用的架构。
* 栈的存取速度回比堆较快，因为他的设计模式让他申请内存和释放内存更快（只需要栈指针增加或者减少）。还有一般栈的内存地址都是连续的，另外栈中的每个字节都会经常被重复使用，往往会被缓存在处理器的缓存中。

# 引用
[stack heap](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)
[memory layout of c program](http://www.geeksforgeeks.org/memory-layout-of-c-program/)
[data segmnet](http://en.wikipedia.org/wiki/Data_segment)
[code segment](http://en.wikipedia.org/wiki/Code_segment)
[bbs](http://en.wikipedia.org/wiki/.bss)
[stack](https://en.wikipedia.org/wiki/Heap_(data_structure))
[heap](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
[virtual memory](https://en.wikipedia.org/wiki/Virtual_memory)
[memory management](https://en.wikipedia.org/wiki/Memory_management)
