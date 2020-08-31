# Lab5：用户程序

之前我们已经实现了内存的管理和内核进程的建立。但是那都是在内核态。

接下来我们将在用户态运行一些程序。

用户程序，也就是我们在计算机系课程里一直在写的那些程序，到底怎样在操作系统上跑起来？

首先需要编译器把用户程序的源代码编译为可以在CPU执行的目标程序，这个目标程序里，既要有执行的代码，又要有关于内存分配的一些信息，告诉我们应该怎样为这个程序分配内存。

我们先不考虑怎样在ucore里运行编译器，只考虑ucore如何把编译好的用户程序运行起来。这需要给它分配一些内存，把程序代码加载进来，建立一个进程，然后通过调度让这个用户进程开始执行。