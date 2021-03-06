# 安装硬件模拟器

## 下载安装QEMU

我们也像安装编译器一样，去sifive的官网[下载](https://static.dev.sifive.com/dev-tools/riscv-qemu-4.2.0-2020.04.0-x86_64-linux-ubuntu14.tar.gz?_ga=2.19251771.2004657256.1596986275-1484936641.1596986275)Ubuntu系统的riscv-qemu，里面的很多设置都已经设置好了的。在下载完，然后解压到之前的riscv文件夹下，把里面的bin文件夹加入到环境变量。修改完记得重启虚机噢。（这次熟练多了吧~）。

## 使用OpenSBI

新版 Qemu 中内置了 OpenSBI 固件（firmware），它主要负责在操作系统运行前的硬件初始化和加载操作系统的功能。我们使用以下命令尝试运行一下：

```text
qemu-system-riscv64   --machine virt   --nographic   --bios default
```

如果成功的话，就可以看到。

```text
OpenSBI v0.5 (Oct  9 2019 12:03:04)
   ____                    _____ ____ _____
  / __ \                  / ____|  _ \_   _|
 | |  | |_ __   ___ _ __ | (___ | |_) || |
 | |  | | '_ \ / _ \ '_ \ \___ \|  _ < | |
 | |__| | |_) |  __/ | | |____) | |_) || |_
  \____/| .__/ \___|_| |_|_____/|____/_____|
        | |
        |_|

Platform Name          : QEMU Virt Machine
Platform HART Features : RV64ACDFIMSU
Platform Max HARTs     : 8
Current Hart           : 0
Firmware Base          : 0x80000000
Firmware Size          : 116 KB
Runtime SBI Version    : 0.2

PMP0: 0x0000000080000000-0x000000008001ffff (A)
PMP1: 0x0000000000000000-0xffffffffffffffff (A,R,W,X)
```

可以看到我们已经在 `qemu-system-riscv64` 模拟的 `virt machine` 硬件上将 `OpenSBI` 这个固件 跑起来了。Qemu 可以使用 `Ctrl+a` 再按下 `x` 退出（注意要松开`Ctrl`再单独按`x`）。

如果无法正常使用 Qemu，可以尝试下面这个命令。

```text
$ sudo sysctl vm.overcommit_memory=1
```

