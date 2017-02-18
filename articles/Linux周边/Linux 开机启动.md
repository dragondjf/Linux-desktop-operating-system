Linux 是如何开机启动的
======

#####1.最初始阶段：

当我们打开计算机电源，计算机会自动从主板的BIOS(Basic Input/Output System)读取其中所存储的程序。这一程序通常知道一些直接连接在主板上的硬件(硬盘，网络接口，键盘，串口，并口)。现在大部分的BIOS允许你从软盘、光盘或者硬盘中选择一个来启动计算机。

下一步，计算机将从你所选择的存储设备中读取起始的512 bytes(比如光盘一开是的512 bytes，如果我们从光盘启动的话)。这512 bytes叫做主引导记录MBR (master boot record)。MBR会告诉电脑从该设备的某一个分区(partition)来装载引导加载程序(boot loader)。Boot loader储存有操作系统(OS)的相关信息，比如操作系统名称，操作系统内核 (kernel)所在位置等。常用的boot loader有GRUB和LILO。

随后，boot loader会帮助我们加载kernel。kernel实际上是一个用来操作计算机的程序，它是计算机操作系统的内核，主要的任务是管理计算机的硬件资源，充当软件和硬件的接口。操作系统上的任何操作都要通过kernel传达给硬件。Windows和Linux各自有自己kernel。狭义的操作系统就是指kernel，广义的操作系统包括kernel以及kernel之上的各种应用。

（Linus Torvalds与其说是Linux之父，不如说是Linux kernel之父。他依然负责Linux kernel的开发和维护。至于Ubuntu, Red Hat, 它们都是基于相同的kernel之上，囊括了不同的应用和界面构成的一个更加完整的操作系统版本。)

实际上，我们可以在多个分区安装boot loader，每个boot loader对应不同的操作系统，在读取MBR的时候选择我们想要启动的boot loader。这就是多操作系统的原理。


**启动顺序：BIOS -> MBR -> boot loader -> kernel**

 

#####2. kernel

如果我们加载的是Linux kernel，Linux kernel开始工作。kernel会首先预留自己运行所需的内存空间，然后通过驱动程序(driver)检测计算机硬件。这样，操作系统就可以知道自己有哪些硬件可用。随后，kernel会启动一个init进程。它是Linux系统中的1号进程(Linux系统没有0号进程)。到此，kernel就完成了在计算机启动阶段的工作，交接给init来管理。

 

**启动顺序： kernel -> init process**

 

#####3. init process

(根据boot loader的选项，Linux此时可以进入单用户模式(single user mode)。在此模式下，初始脚本还没有开始执行，我们可以检测并修复计算机可能存在的错误)

随后，init会运行一系列的**初始脚本**(startup scripts)，这些脚本是Linux中常见的shell scripts。这些脚本执行如下功能：

    设置计算机名称，时区，检测文件系统，挂载硬盘，清空临时文件，设置网络……

当这些初始脚本完成之后，操作系统已经完全准备好了，只是，还没有人可以登录！！！init会给出登录(login)对话框，或者是图形化的登录界面。

 

#####4. 总结

**启动顺序：BIOS -> MBR -> boot loader -> kernel -> init process -> login**


#####5.【参考】
+ [vamei博客](http://www.cnblogs.com/vamei)
