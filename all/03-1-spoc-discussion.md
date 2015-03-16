# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
>最先匹配：优势，简单，在高地址空间有大块的空闲分区，缺点：存在大量外部碎片，在分配大块空间时需要较多时间。  
最差匹配：优势，中等大小的分配较多时效果最好，避免出现太多的小碎片，缺点：释放分区较慢，存在很多外部碎片，后续难以分配大的分区。  
最佳匹配：优点：大部分分配的尺寸较小时，效果很好， 即可以避免大的空闲分区被拆分，可减小外部随便大小。缺点：仍存在  
大量小的外部碎片，释放分区较慢。  
buddy systemm： 优点：可以避免大的空闲分区被拆分，可减小外部随便大小，类似于最佳算法，不容易产生大量小的外部碎片。缺点：  
释放分区较慢，实现稍复杂。
提出一个连续内存分配算法：内存最小单元变大，比如由一字节变为1024字节，分配时以1024字节为单位进行分配，在此基础上利用最佳分配算法进行分配，这样不存在特别小的外部碎片。
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- [x]  

>  

## 小组思考题

请参考ucore lab2代码，采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)来实现，给出你的设思路，并给出测试用例。 (spoc)
>#include <iostream>  
using namespace std;  
struct block  
{  
	int size;  
	bool occupy;  
	block *last;  
	block *next;  
};  
block * head = new block;  
block * malloc(int len)  
{  
	int max=-0;  
	block * temp= head;  
	block * candi = head;  
	do{  
		if(((temp->size)>max)&&(temp->occupy==false))  
		{  
				max = temp->size;  
				candi = temp;  
			 
		}  
			temp = temp->next;  
	}while(temp != NULL);  
	if(max<len)  
	{  
			cout<<"内存不足"<<endl;  
			return NULL;  
	}  
	else  
	{  
		block * new_block= new block;  
		new_block->size = (candi -> size)-len;  
		new_block->occupy = false;  
		new_block->last = candi;  
		new_block->next = candi->next;  
		candi-> size = len;  
		candi ->occupy = true;  
		candi ->next = new_block;  
		return candi;
	}  
}  
void free(block * bl)  
{  
	bl->occupy = false;  
	if((bl->next)!=NULL)  
	{  
		if((bl->next)->occupy ==false)  
		{  
			bl->size = ( bl->size ) + (bl -> next)->size;  
			bl->next = (bl -> next)->next;  
		}  
    }  
    if((bl->last)!=NULL)  
    {  
		if((bl->last)->occupy == false)  
		{  
			bl->size = ( bl->size ) + (bl -> last)->size;  
			bl->last = (bl -> last)->last;  
			if(bl->last==NULL)  
				head = bl;  
		}  
    }		 	 		
}  
void inite(block *head)  
{  
	head->size = 4096;  
	head->occupy = false;  
	head->last= NULL;  
	head->next = NULL;  
}  
void print_pmm()  
{  
	block * temp = head;  
	while (temp!=NULL)  
	{  
		cout<<(temp->size)<<" ";  
		if(temp->occupy)	  
		cout<<"占用"<<endl;  
		else  
		cout<<"空闲"<<endl;  
		temp = temp->next;  
	}   
}  
int main()  
{  
	inite(head);  
	cout<<"内存空间大小为"<<head->size<<endl;  
	cout<<"启动内存为200的进程"<<endl;  
	cout<<endl;  
	malloc(200);  
	print_pmm();  
	cout<<"启动内存为100的进程"<<endl;  
	cout<<endl;  
	malloc(100);  
	print_pmm();  
	cout<<"启动内存为1000的进程"<<endl;  
	cout<<endl;  
	malloc(1000);  
	print_pmm();  
	cout<<"启动内存为500的进程"<<endl;  
	cout<<endl;  
	malloc(500);  
	print_pmm();  
	cout<<"关闭内存为100的进程"<<endl;  
	cout<<endl;  
	free((head->next));  
	print_pmm();  
	cout<<"启动内存为50的进程"<<endl;  
	cout<<endl;  
	malloc(50);  
	print_pmm();  
	cout<<"关闭内存为1000的进程"<<endl;  
	cout<<endl;  
	free((head->next->next));  
	print_pmm();  
	system("pause");  
	return 0;  
}  
		

--- 

## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。

## “连续内存分配”与视频相关的课堂练习

### 5.1 计算机体系结构和内存层次
MMU的工作机理？

- [x]  

>  http://en.wikipedia.org/wiki/Memory_management_unit

L1和L2高速缓存有什么区别？

- [x]  

>  http://superuser.com/questions/196143/where-exactly-l1-l2-and-l3-caches-located-in-computer
>  Where exactly L1, L2 and L3 Caches located in computer?

>  http://en.wikipedia.org/wiki/CPU_cache
>  CPU cache

### 5.2 地址空间和地址生成
编译、链接和加载的过程了解？

- [x]  

>  

动态链接如何使用？

- [x]  

>  


### 5.3 连续内存分配
什么是内碎片、外碎片？

- [x]  

>  

为什么最先匹配会越用越慢？

- [x]  

>  

为什么最差匹配会的外碎片少？

- [x]  

>  

在几种算法中分区释放后的合并处理如何做？

- [x]  

>  

### 5.4 碎片整理
一个处于等待状态的进程被对换到外存（对换等待状态）后，等待事件出现了。操作系统需要如何响应？

- [x]  

>  

### 5.5 伙伴系统
伙伴系统的空闲块如何组织？

- [x]  

>  

伙伴系统的内存分配流程？

- [x]  

>  

伙伴系统的内存回收流程？

- [x]  

>  

struct list_entry是如何把数据元素组织成链表的？

- [x]  

>  



