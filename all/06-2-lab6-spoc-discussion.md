# lab6 spoc 思考题

- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。


## 个人思考题

### 总体介绍

 - ucore中的调度点在哪里，完成了啥事？
 - 进程控制块中与调度相关的字段有哪些？
 - ucore的就绪队列数据结构在哪定义？在哪进行修改？
 - ucore的等待队列数据结构在哪定义？在哪进行修改？

### 调度算法支撑框架

 - 调度算法支撑框架中的各个函数指针的功能是啥？会被谁在何种情况下调用？
 - 调度函数schedule()的调用函数分析，可以了解进程调度的原因。请分析ucore中所有可能的调度位置，并说明可能的调用原因。
 
### 时间片轮转调度算法

 - 时间片轮转调度算法是如何基于调度算法支撑框架实现的？
 - 时钟中断如何调用RR_proc_tick()的？

### stride调度算法

 - stride调度算法的思路？ 
 - stride算法的特征是什么？
 - stride调度算法是如何避免stride溢出问题的？
 - 无符号数的有符号比较会产生什么效果？
 - 什么是斜堆(skew heap)?

## 小组练习与思考题

### (1)(spoc) 理解调度算法支撑框架的执行过程

即在ucore运行过程中通过`cprintf`函数来完整地展现出来多个进程在调度算法和框架的支撑下，在相关调度点如何动态调度和执行的细节。(约全面细致约好)

请完成如下练习，完成代码填写，并形成spoc练习报告
> 需写练习报告和简单编码，完成后放到git server 对应的git repo中

▾
Note History:
note
52 views
lab6 spoc 提交处

 

课堂问答
Updated 10 days ago by Yu Chen
followup discussions
for lingering questions and comments
Resolved Unresolved
[谢晓晖]
谢晓晖 10 days ago

鲁逸沁 2012011314

谢晓晖 2012011315
[谢晓晖]
谢晓晖 10 days ago

https://github.com/THUxiexiaohui/ucore_lab/tree/master/labcodes_answer/lab6_result
Reply to this followup discussion
Resolved Unresolved
[TTHJ]
TTHJ 10 days ago

计22 黄杰 2012011272

计22 袁源 2012011294

计24 杜鹃 2012011354
[TTHJ]
TTHJ 10 days ago

https://github.com/THUHJ/ucore_lab/tree/master/related_info/lab6_spoc_discuss
Reply to this followup discussion
Resolved Unresolved
[zhoujie15]
zhoujie15 10 days ago

 

---------------------- Scehdule at cpu_idle proc 0 --------------------------
---------------------- Schedule at do_wait proc 1 --------------------------
kernel_execve: pid = 2, name = "exit".
I am the parent. Forking the child...
I am parent, fork a child pid 3
I am the parent, waiting now..
---------------------- Schedule at do_wait proc 1 --------------------------
I am the child.
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at trap before proc 3--------------------------
---------------------- Schedule at trap after proc 3--------------------------
---------------------- Schedule at do_exit proc 3--------------------------
waitpid 3 ok.
exit pass.
---------------------- Schedule at do_exit proc 2--------------------------
---------------------- Schedule at init_main --------------------------

计21 周界 2012011394

计21 张梦豪 2012011401

计21 马晓彬 2012011402
Reply to this followup discussion
Resolved Unresolved
[ZHOU Shengkai]
ZHOU Shengkai 10 days ago

王振 2012011397

韦福超 2012011392

周圣凯2012011342

 

++ setup timer interrupts
sched_class_proc_tick:pid = 0, stride = 0, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 0, stride = 0, priority = 0
sched_class_dequeue: pid = 1, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 1, stride = 2147483647, priority = 0
wakeup_proc: 2
sched_class_enqueue: pid = 2, stride = 0, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_dequeue: pid = 2, stride = 2147483647, priority = 0
kernel_execve: pid = 2, name = "priority".
wakeup_proc: 3
sched_class_enqueue: pid = 3, stride = 0, priority = 0
wsched_class_proc_tick:pid = 2, stride = 2147483647, priority = 6
akeup_proc: 4
sched_class_enqueue: pid = 4, stride = 0, priority = 0
wakeup_proc: 5
sched_class_enqueue: pid = 5, stride = 0, priority = 0
wakeup_proc: 6
sched_class_enqueue: pid = 6, stride = 0, priority = 0
wakeup_proc: 7
sched_class_enqueue: pid = 7, stride = 0, priority = 0
main: fork ok,now need to wait pids.
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_dequeue: pid = 7, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 7, stride = 2147483647, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 2147483647, priority = 5
sched_class_dequeue: pid = 6, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 6, stride = 2147483647, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 2147483647, priority = 4
sched_class_dequeue: pid = 5, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 5, stride = 2147483647, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 2147483647, priority = 3
sched_class_dequeue: pid = 4, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 4, stride = 2147483647, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 2147483647, priority = 2
sched_class_dequeue: pid = 3, stride = 2147483647, priority = 0
sched_class_proc_tick:pid = 3, stride = 2147483647, priority = 0
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 3, stride = 2147483647, priority = 1
sched_class_dequeue: pid = 3, stride = 4294967294, priority = 1
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 3, stride = 4294967294, priority = 1
sched_class_dequeue: pid = 5, stride = 2863311529, priority = 3
sched_class_proc_tick:pid = 5, stride = 2863311529, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 2863311529, priority = 3
sched_class_dequeue: pid = 4, stride = 3221225470, priority = 2
sched_class_proc_tick:pid = 4, stride = 3221225470, priority = 2
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 3221225470, priority = 2
sched_class_dequeue: pid = 6, stride = 2684354558, priority = 4
sched_class_proc_tick:pid = 6, stride = 2684354558, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 2684354558, priority = 4
sched_class_dequeue: pid = 7, stride = 2576980376, priority = 5
sched_class_proc_tick:pid = 7, stride = 2576980376, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 2576980376, priority = 5
sched_class_dequeue: pid = 7, stride = 3006477105, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 3006477105, priority = 5
sched_class_dequeue: pid = 6, stride = 3221225469, priority = 4
sched_class_proc_tick:pid = 6, stride = 3221225469, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 3221225469, priority = 4
sched_class_dequeue: pid = 5, stride = 3579139411, priority = 3
sched_class_proc_tick:pid = 5, stride = 3579139411, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 3579139411, priority = 3
sched_class_dequeue: pid = 7, stride = 3435973834, priority = 5
sched_class_proc_tick:pid = 7, stride = 3435973834, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 3435973834, priority = 5
sched_class_dequeue: pid = 6, stride = 3758096380, priority = 4
sched_class_proc_tick:pid = 6, stride = 3758096380, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 3758096380, priority = 4
sched_class_dequeue: pid = 4, stride = 4294967293, priority = 2
sched_class_proc_tick:pid = 4, stride = 4294967293, priority = 2
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 4294967293, priority = 2
sched_class_dequeue: pid = 7, stride = 3865470563, priority = 5
sched_class_proc_tick:pid = 7, stride = 3865470563, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 3865470563, priority = 5
sched_class_dequeue: pid = 5, stride = 4294967293, priority = 3
sched_class_proc_tick:pid = 5, stride = 4294967293, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 4294967293, priority = 3
sched_class_dequeue: pid = 6, stride = 4294967291, priority = 4
sched_class_proc_tick:pid = 6, stride = 4294967291, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 4294967291, priority = 4
sched_class_dequeue: pid = 7, stride = 4294967292, priority = 5
sched_class_proc_tick:pid = 7, stride = 4294967292, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 4294967292, priority = 5
sched_class_dequeue: pid = 6, stride = 536870906, priority = 4
sched_class_proc_tick:pid = 6, stride = 536870906, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 536870906, priority = 4
sched_class_dequeue: pid = 7, stride = 429496725, priority = 5
sched_class_proc_tick:pid = 7, stride = 429496725, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 429496725, priority = 5
sched_class_dequeue: pid = 5, stride = 715827879, priority = 3
sched_class_proc_tick:pid = 5, stride = 715827879, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 715827879, priority = 3
sched_class_dequeue: pid = 4, stride = 1073741820, priority = 2
sched_class_proc_tick:pid = 4, stride = 1073741820, priority = 2
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 1073741820, priority = 2
sched_class_dequeue: pid = 3, stride = 2147483645, priority = 1
sched_class_proc_tick:pid = 3, stride = 2147483645, priority = 1
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 3, stride = 2147483645, priority = 1
sched_class_dequeue: pid = 7, stride = 858993454, priority = 5
sched_class_proc_tick:pid = 7, stride = 858993454, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 858993454, priority = 5
sched_class_dequeue: pid = 6, stride = 1073741817, priority = 4
sched_class_proc_tick:pid = 6, stride = 1073741817, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 1073741817, priority = 4
sched_class_dequeue: pid = 5, stride = 1431655761, priority = 3
sched_class_proc_tick:pid = 5, stride = 1431655761, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 1431655761, priority = 3
sched_class_dequeue: pid = 7, stride = 1288490183, priority = 5
sched_class_proc_tick:pid = 7, stride = 1288490183, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 1288490183, priority = 5
sched_class_dequeue: pid = 6, stride = 1610612728, priority = 4
sched_class_proc_tick:pid = 6, stride = 1610612728, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 1610612728, priority = 4
sched_class_dequeue: pid = 4, stride = 2147483643, priority = 2
sched_class_proc_tick:pid = 4, stride = 2147483643, priority = 2
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 2147483643, priority = 2
sched_class_dequeue: pid = 7, stride = 1717986912, priority = 5
sched_class_proc_tick:pid = 7, stride = 1717986912, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 1717986912, priority = 5
sched_class_dequeue: pid = 5, stride = 2147483643, priority = 3
sched_class_proc_tick:pid = 5, stride = 2147483643, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 2147483643, priority = 3
sched_class_dequeue: pid = 6, stride = 2147483639, priority = 4
sched_class_proc_tick:pid = 6, stride = 2147483639, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 2147483639, priority = 4
sched_class_dequeue: pid = 7, stride = 2147483641, priority = 5
sched_class_proc_tick:pid = 7, stride = 2147483641, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 2147483641, priority = 5
sched_class_dequeue: pid = 6, stride = 2684354550, priority = 4
sched_class_proc_tick:pid = 6, stride = 2684354550, priority = 4
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 6, stride = 2684354550, priority = 4
sched_class_dequeue: pid = 7, stride = 2576980370, priority = 5
sched_class_proc_tick:pid = 7, stride = 2576980370, priority = 5
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 7, stride = 2576980370, priority = 5
sched_class_dequeue: pid = 5, stride = 2863311525, priority = 3
sched_class_proc_tick:pid = 5, stride = 2863311525, priority = 3
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 5, stride = 2863311525, priority = 3
sched_class_dequeue: pid = 4, stride = 3221225466, priority = 2
sched_class_proc_tick:pid = 4, stride = 3221225466, priority = 2
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_class_enqueue: pid = 4, stride = 3221225466, priority = 2
sched_class_dequeue: pid = 3, stride = 4294967292, priority = 1
sched_class_proc_tick:pid = 3, stride = 4294967292, priority = 1
schedule!!!!!!!!!!!!!!!!!!!!!!!!!!
sched_cla


### 练习用的[lab6 spoc exercise project source code](https://github.com/chyyuu/ucore_lab/tree/master/labcodes_answer/lab6_result)

