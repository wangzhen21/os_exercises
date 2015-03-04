# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- [x]  

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm  
可以读懂大部分汇编语言。  
有部分语言不理解；inb $0x64, %al； outb %al；

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- [x]  

>   虽然学过计算机组成原理，但是目前仍然对虚存管理，cache、内存、硬盘的一致性保持，进程、线程的切换存有疑问。


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- [x]  

>   1、对实验环境的熟悉程度，如果工具不会用的话我会主动请教会了的同学。  
2、对知识点的理解不充分的情况下可能会找同学请教。  
3、坚决不抄袭。  

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- [x]  

>   段机制启动、页机制未启动：逻辑地址->线性地址=物理地址；      段机制和页机制启动：逻辑地址->段机制处理->线性地址->物理地址。

了解函数调用栈对lab实验有何帮助？
- [x]  

>   知道操作系统对于函数的调用分配的具体操作顺序，可以从细节上了解计算机函数调用机制。

你希望从lab中学到什么知识？
- [x]  

>   了解操作系统的原理，深入理解操作系统的工作细节。  
了解目前不同操作系统有差别的原因   

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- [x]  

> 搭建过程按照普通方法，在VirtualBox上运行系统  
遇到了无法解压OS_MOOC的问题，使用好压解压解决了此问题。

熟悉基本的git命令行操作命令，从github上
的 http://www.github.com/chyyuu/ucore_lab 下载
ucore lab实验
- [x]  

> 已尝试调试。

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- [x]   

>  已完成

对于如下的代码段，请说明”：“后面的数字是什么含义
```
/* Gate descriptors for interrupts and traps */
struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
};
```

- [x]  

> 下一个的数字表示对应的位数。

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```

如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？

- [x]  

> 2

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
- [x]  

>  对链表的系列操作，含有初始化，添加，特定位置添加(add_before、add_after)，删除(del)，删除并初始化(del_init)，清空(empty)，链表下一个(next)，前缀(prev)。

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  

---
