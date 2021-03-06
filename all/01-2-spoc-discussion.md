# 操作系统概述思考题
>姓名：王振  学号2012011397      小组成员：王振 张梦豪
## 个人思考题

---

分析你所认识的操作系统（Windows、Linux、FreeBSD、Android、iOS）所具有的独特和共性的功能？
- [x]  
>独特功能:  
>Windows:图形界面良好，整合常用应用软件
>Linux:开源  
>FreeBSD:具有可调整的动态优先级抢占式多任务能力、是多用户操作系统。  
>安卓:开放性强、硬件选择丰富  
>iOS：占用系统资源少。  
>共性：  
>都有五大功能，微处理器管理功能、内存管理功能、外部设备管理功能、文件管理功能、进程管理功能。   

请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
- [x]  

>并发性 并行性是指两个或多个事件在同一时刻发生，而并发性是指两个或多个事件在同一时间间隔内发生。  
>共享性 所谓共享是指系统中的资源可供内存中多个并发执行的进程（线程）共同使用。  
>虚拟性 所谓虚拟是指通过某项技术把一个物理实体变为若干个逻辑上的对应物。  
>异步性 操作系统允许多个并发进程共享资源，使得每个进程的运行过程受到其他进程制约，使进程的执行不是一气呵成，而是以停停走走的方式运行  

请给出你觉得的更准确的操作系统的定义？
- [x]  

>操作系统（Operating System,简称OS）是管理电脑硬件与软件资源的程序,同时也是计算机系统的内核与基石.操作系统是控制其他程序运行,管理系统资源并为用户提供操作界面的系统软件的集合.   

你希望从操作系统课学到什么知识？
- [x]  
 
>1、掌握操作系统的基本概念、基本结构。  
>2、深入理解操作系统内部以及基本原理。

---

## 小组讨论题

---

目前的台式PC机标准配置和价格？
- [x]  

>台式PC机标准配置：8核CPU。2G独显,750G硬盘,4G内存，  
价格：4000元

你理解的命令行接口和GUI接口具有哪些共性和不同的特征？
- [x]  

>共性：  
都是人机交互的常用方式。    
>不同特性：  
>GUI接口的优势是学习成本低；不足是在熟练的情况下操作速度不如命令行。  
>命令行接口优势是定制更灵活，熟练情况下操作速度快；不足就是学习成本巨高无比。  
>对于业余的用户，使用一些不常用的软件，GUI 能够降低学习成本。对于深度的用户，使用一些频繁使用的软件，用命令行可以提高操作效率

为什么现在的操作系统基本上用C语言来实现？
- [x]  

> 在开发时间和软件效率之间取一个平衡点，C语言就满足了你人们这一要求，具体来讲如下。  
1、C语言可移植性强。  
2、C语言是编译型语言。  
3、容易嵌入汇编。  
4、兼容性好，不同编译器编译的符号基本相同，容易链接。  
5、大多数编译器用C/C++写的。  
6、C语言开发真众多。  
7、这种语言足够成熟。
为什么没有人用python，java来实现操作系统？
- [x]  

为什么没有人用python，java来实现操作系统？

>  1、python和java运行效率低下，难以满足操作系统的性能要求，故没有人使用python和java来实现操作系统。  
2、Java不容易嵌入汇编代码。


请评价用C++来实现操作系统的利弊？
- [x]  

>  与C相比，C++性能上还略逊一筹，我认为集中表现在一下两点。  
1、兼容性不是很好，不同编译器编译的符号差别较大。  
2、C++开发操作系统的材料不够完善。


---

## 开放思考题

---

请评价微内核、单体内核、外核（exo-kernel）架构的操作系统的利弊？
- [x]  

>微内核：利：1、模块化的方式设计使设计者只需要关注自己的功能模块  
2、系统运行时，可根据需要释放计算机的资源。  
弊：在性能上有缺陷，因为一个进程需要使用某个功能模块时，需要通过消息机制来传输，这会消耗大量的时间。  
单体内核：利：相比于微内核，性能有较大提高。  
弊：1、为了适应新的硬件驱动，每次都需要更新内核。  
2、运行时自身占用大量的物理资源。  
外核操作系统：利：弥补单体内核操作系统硬件资源基本上由内核直接管理和保护，在执行效率、维护、以及应用扩展上都存在着一些不足。  
弊：外核将资源保护及其管理分割开来，实现起来较为复杂，


请评价用LISP,OCcaml, GO, D，RUST等实现操作系统的利弊？
- [x]  

>  对LISP,OCcaml, GO, D，RUST未有接触，在网上查询后有以下结果：  
Go外用CGO可以直接include C header并直接在代码中引用C符号， 
调用C库很方便，也算是非常大的优点。GO比较适合quick和dirty的处理底层事务，但由于interface的implicit等局
限用来管理大型工程可能会比较麻烦。  
RUST在大型工程领域有很好的应用。   

进程切换的可能实现思路？
- [x]  

>  通过中断、保存现场、恢复等操作来实现进程的切换
 
计算机与终端间通过串口通信的可能实现思路？
- [x]  

>  主要依靠进程的切换来实现串口通信。

为什么微软的Windows没有在手机终端领域取得领先地位？
- [x]  

>  1、微软没有第一时间占据智能手机市场的优势地位。  
2、适用的软件并不丰富，导致用户流失。

你认为未来（10年内）的操作系统应该具有什么样的特征和功能？
- [x]  

>   1、开源化。这样可以聚集大家的力量，打破组织边界，创造出更高质量的操作系统。  
2、专用化。未来越来越多的领域需要满足特殊需求的专用操作系统。  
3、小型化。为了适应特定的应用领域，未来操作系统必须向功能小型化发展。  


---
