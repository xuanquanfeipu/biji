##### OS笔记 第三讲 启动，中断，异常和系统调用

1.BIOS和启动流程

BIOS是在固件的。实模式下不做地址转换，CPU的20位地址线只能访问低1M的内存（讨论8086机器）。其中640K到1M的地方是BIOS固件，在ROM的，开始时CPU进入CS：IP=0xf000：fff0，是把段寄存器CS左移4位加IP=0xffff0（20位地址）。

BIOS提供磁盘，键盘，显示器读写功能。x86的BIOS提供的功能只能在实模式下访问。实模式都是16位的。

然后BIOS将加载程序从磁盘的引导扇区（512字节）加载到0x7c00，跳转到0000：7c00。加载程序再加载OS。同时也可以有设置菜单，包括启动顺序，或者选择从哪个硬盘外设上启动。（但不是硬盘内的双系统）

主引导记录是在主引导分区的，一共512字节。**加载程序**只有446字节，剩下64字节是分区表（4个）和0x55AA。加载程序只能检查一下分区正确性，然后加载并跳转**活动分区**上面的**引导程序**。*活动分区只有一个，但是每个主分区都可以引导OS，OS选择菜单是在活动分区里的.* 主引导记录不认识文件系统格式，所以必须在活动分区内加载。

活动分区的引导扇区：JMP 	文件卷结构 启动代码 0x55AA。

JMP的地址和CPU相关。

启动代码再加载OS的加载程序，或者提供选择OS的菜单。这个启动代码是认识文件系统格式的，可以同时有加载windows或linux内核的能力。

BIOS是一套**标准**，这样同一个磁盘就可以在所有电脑上启动。