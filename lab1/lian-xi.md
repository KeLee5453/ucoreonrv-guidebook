# 练习

## 练习1：描述处理中断异常的流程

像LAB0.5里的执行流一样描述ucore是如何处理中断异常的。从异常的产生开始。

## 练习2：对于任何中断，都需要保存所有寄存器吗？为什么？

## 练习3：触发、捕获、处理异常

编程：在任意位置触发一条非法指令异常（如：mret），在 `kern/trap/trap.c`的异常处理函数中捕获，并对其进行处理，简单输出异常类型和指令即可。
