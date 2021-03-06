# 链接脚本

gnu工具链中，包含一个链接器`ld`

如果你很好奇，可以看[linker script的详细语法](http://www.scoberlin.de/content/media/http/informatik/gcc_docs/ld_3.html)

链接器的作用是把输入文件\(往往是 .o文件\)链接成输出文件\(往往是elf文件\)。一般来说，输入文件和输出文件都有很多section, 链接脚本\(linker script\)的作用，就是描述怎样把输入文件的section映射到输出文件的section, 同时规定这些section的内存布局。

如果你不提供链接脚本，ld会使用默认的一个链接脚本，这个默认的链接脚本适合链接出一个能在现有操作系统下运行的应用程序，但是并不适合链接一个操作系统内核。你可以通过`ld --verbose`来查看默认的链接脚本。

{% hint style="info" %}
要打开代码看噢，这里没有哒！
{% endhint %}

我们在链接脚本里把程序的入口点定义为`kern_entry`, 那么我们的程序里需要有一个名称为`kern_entry`的符号。我们在`kern/init/entry.S`编写了一段汇编代码, 作为整个内核的入口点。

```text
#include <mmu.h>
#include <memlayout.h>

    .section .text,"ax",%progbits
    .globl kern_entry
kern_entry:
    la sp, bootstacktop

    tail kern_init
 #调用kern_init, 这是我们要用C语言编写的一个函数, tail是riscv伪指令，作用相当于调用函数（跳转）
.section .data
    # .align 2^12
    .align PGSHIFT
    .global bootstack
bootstack:
    .space KSTACKSIZE
    .global bootstacktop
bootstacktop:
```

里面有很多符号和指令，定义的符号，应该知道去哪找吧（偷偷告诉你吧，在头文件呀！）。关于指令呢，就需要自己去查阅手册咯。如果看不懂汇编代码结构的，自己查资料噢~不要怀着疑惑往下做。会越来越懵。

诶诶，看到这，下一步该干嘛，该去找哪个呢，想一想噢。

哎鸭！调用了唯一的一个函数，当然是去找函数啦。这个函数在干嘛呢？不好奇么。那就往下看。

