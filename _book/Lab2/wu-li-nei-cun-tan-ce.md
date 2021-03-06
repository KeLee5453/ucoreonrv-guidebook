# 物理内存探测

操作系统怎样知道物理内存所在的那段物理地址呢？在 RISC-V 中，这个一般是由 bootloader ，即 OpenSBI 来完成的。它来完成对于包括物理内存在内的各外设的扫描，将扫描结果以 DTB\(Device Tree Blob\) 的格式保存在物理内存中的某个地方。随后 OpenSBI 会将其地址保存在 `a1` 寄存器中，给我们使用。

这个扫描结果描述了所有外设的信息，当中也包括 Qemu 模拟的 RISC-V 计算机中的物理内存。

> \[!TIP\|style:flat\|label:Qemu 模拟的 RISC-V virt 计算机中的物理内存\]
>
> 通过查看[virt.c](https://github.com/qemu/qemu/blob/master/hw/riscv/virt.c)的**virt\_memmap\[\]**的定义，可以了解到 Qemu 模拟的 RISC-V virt 计算机的详细物理内存布局。可以看到，整个物理内存中有不少内存空洞（即含义为**unmapped**的地址空间），也有很多外设特定的地址空间，现在我们看不懂没有关系，后面会慢慢涉及到。目前只需关心最后一块含义为**DRAM**的地址空间，这就是 OS 将要管理的 128MB 的内存空间。
>
> | 起始地址 | 终止地址 | 含义 |
> | :--- | :--- | :--- |
> | 0x0 | 0x100 | QEMU VIRT\_DEBUG |
> | 0x100 | 0x1000 | unmapped |
> | 0x1000 | 0x12000 | QEMU MROM \(包括 hard-coded reset vector; device tree\) |
> | 0x12000 | 0x100000 | unmapped |
> | 0x100000 | 0x101000 | QEMU VIRT\_TEST |
> | 0x101000 | 0x2000000 | unmapped |
> | 0x2000000 | 0x2010000 | QEMU VIRT\_CLINT |
> | 0x2010000 | 0x3000000 | unmapped |
> | 0x3000000 | 0x3010000 | QEMU VIRT\_PCIE\_PIO |
> | 0x3010000 | 0xc000000 | unmapped |
> | 0xc000000 | 0x10000000 | QEMU VIRT\_PLIC |
> | 0x10000000 | 0x10000100 | QEMU VIRT\_UART0 |
> | 0x10000100 | 0x10001000 | unmapped |
> | 0x10001000 | 0x10002000 | QEMU VIRT\_VIRTIO |
> | 0x10002000 | 0x20000000 | unmapped |
> | 0x20000000 | 0x24000000 | QEMU VIRT\_FLASH |
> | 0x24000000 | 0x30000000 | unmapped |
> | 0x30000000 | 0x40000000 | QEMU VIRT\_PCIE\_ECAM |
> | 0x40000000 | 0x80000000 | QEMU VIRT\_PCIE\_MMIO |
> | 0x80000000 | 0x88000000 | DRAM 缺省 128MB，大小可配置 |

