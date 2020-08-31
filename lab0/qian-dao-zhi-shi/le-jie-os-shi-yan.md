# 了解OS实验

写一个操作系统难吗？别被现在上百万行的Linux和Windows操作系统吓倒。当年Thompson趁他老婆带着小孩度假留他一人在家时，写了UNIX；当年Linus还是一个21岁大学生时完成了Linux雏形。站在这些巨人的肩膀上，我们能否也尝试一下做“巨人”的滋味呢？

## 实验内容

那我们准备如何一步一步来实现ucore呢？根据一个操作系统的设计实现过程，我们可以有如下的实验步骤：

1. 启动操作系统的bootloader:OpenSBI。了解操作系统启动前的状态和要做的准备工作，了解运行操作系统的硬件支持，操作系统如何加载到内存中，理解两类中断--“外设中断”，“陷阱中断”等；
2. 物理内存管理子系统。用于理解RISC-V分段/分页模式，了解操作系统如何管理物理内存；
3. 虚拟内存管理子系统。通过页表机制和换入换出（swap）机制，以及中断-“故障中断”、缺页故障处理等，实现基于页的内存替换算法；
4. 内核线程子系统。用于了解如何创建相对与用户进程更加简单的内核态线程，如果对内核线程进行动态管理等；
5. 用户进程管理子系统。用于了解用户态进程创建、执行、切换和结束的动态管理过程，了解在用户态通过系统调用得到内核态的内核服务的过程；
6. 处理器调度子系统。用于理解操作系统的调度过程和调度算法；
7. 同步互斥与进程间通信子系统。了解进程间如何进行信息交换和共享，并了解同步互斥的具体实现以及对系统性能的影响，研究死锁产生的原因，以及如何避免死锁；
8. 文件系统。了解文件系统的具体实现，与进程管理等的关系，了解缓存对操作系统IO访问的性能改进，了解虚拟文件系统（VFS）、buffer cache和disk driver之间的关系。

其中每个开发步骤都是建立在上一个步骤之上的，就像搭积木，从一个一个小木块，最终搭出来一个小房子。在搭房子的过程中，完成从理解操作系统原理到实践操作系统设计与实现的探索过程。这个房子最终的建筑架构和建设进度如下图所示：

![ucore&#x7CFB;&#x7EDF;&#x7ED3;&#x6784;&#x56FE;](https://chyyuu.gitbooks.io/ucore_os_docs/content/lab0_figs/image001.png)

## 开发OS实验的步骤

本次OS实验大致可以通过如下过程就可以完成使用：

1. 学习相关理论知识
2. 建立LAB实验环境，采用VMware虚拟机的最简单方式完成
3. 阅读本LAB实验指导书，了解实验要求
4. [下载](le-jie-os-shi-yan.md)源码
5. 进入各个OS实验工程目录 例如：`cd labcodes/lab1`
6. 根据实验要求阅读并修改代码
7. 编译源码 例如执行：`make`
8. 如果不通过则返回步骤3
9. 如果实现不正确（即看到步骤6的输出存在不是OK的情况）则返回3
10. 如果实现基本正确（即看到6的输出都是OK）则`push`到自己github仓库
11. 编写实验报告，发送到助教邮箱，并`push`到github仓库

> \[!NOTE\|style:flat\]
>
> 可以通过`make qemu`让OS实验工程在qemu上运行；可以通过`make debug`或`make debug-nox`命令实现通过gdb远程调试 OS实验工程。
