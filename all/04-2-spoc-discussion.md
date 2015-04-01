#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？
>给进程分配的物理内存增多缺页率反而增加的情况。
7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？
>到目前为止一段时间内访问页面的集合。
9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？
>不断地换入换出导致效率急剧下降。
13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象


(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。
 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
 >  
#include <iostream>  
using namespace std;  
struct page{  
	bool access;  
	bool change;  
	int num;  
}a[4];  
void print_page()  
{  
	for(int i=0;i<4;i++)  
	{  
		cout<<"页号："<<a[i]. num;  
		cout<<"访问位： "<<a[i].access;  
		cout<<"修改位： "<<a[i].change<<endl;  
	}  
}		   
int main()  
{  
	int i;  
	for(i=0;i<4;i++)  	
	{  
		a[i].access=false;  
		a[i].change=false;  
		a[i].num=i;  
	}  
	int page_num;  
	int change_not;  
	int flag=0;  
	int point=0;  
	while(1)  
	{  
		cout<<"输入访问页号"<<endl;  
		cin>>page_num;  
		cout<<"是否修改？(1 or 0)："<<endl;  
		cin>>change_not;  
		for(i=0;i<4;i++)  
		{  
			if(a[i].num==page_num)  
			{  
				flag=1;  
				a[i].access=true;  
				if(change_not)  
				{  
					a[i].change=true;  
				}  
				continue;  
			}  
		}  
		if(flag)  
		{  
			flag=0;  
			print_page();  
		}  
		else  
		{  
			while(1)  
			{  
				if(a[point].access==0&&a[point].change==0)  
				{  
					a[point].access=true;  
					a[point].change=false;  
					a[point].num=page_num;  
					print_page();  
					break;  
				}  
				if(a[point].access==1)  
				{  
						a[point].access=false;  
						point = (point +1)%4;  
						continue;  
				}  
				if(a[point].change==1)  
{  
					point = (point +1)%4;  
					a[point].change==false;  
				}  
			}  
				point = (point +1)%4;  
		}  
	}  
		return 0;  
}  
 
## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
