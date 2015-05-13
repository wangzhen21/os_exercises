# 死锁与IPC(lec 20) spoc 思考题


- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 死锁概念 
 - 尝试举一个生活中死锁实例。
 - 可重用资源和消耗资源有什么区别？

### 可重用和不可撤销；
 - 资源分配图中的顶点和有向边代表什么含义？
 - 出现死锁的4个必要条件是什么？

### 死锁处理方法 
 - 死锁处理的方法有哪几种？它们的区别在什么地方？
 - 安全序列的定义是什么？

### 进程的最大资源需要量小于可用资源与前面进程占用资源的总合；
 - 安全、不安全和死锁有什么区别和联系？

### 银行家算法 
 - 什么是银行家算法？
 - 安全状态判断和安全序列是一回事吗？

### 死锁检测 
 - 死锁检测与安全状态判断有什么区别和联系？

> 死锁检测、安全状态判断和安全序列判断的本质就是资源分配图中的循环等待判断。

### 进程通信概念 
 - 直接通信和间接通信的区别是什么？
  本质上来说，间接通信可以理解为两个直接通信，间接通信中假定有一个永远有效的直接通信方。
 - 同步和异步通信有什么区别？
### 信号和管道 
 - 尝试视频中的信号通信例子。
 - 写一个检查本机网络服务工作状态并自动重启相关服务的程序。
 - 什么是管道？

### 消息队列和共享内存 
 - 写测试用例，测试管道、消息队列和共享内存三种通信机制进行不同通信间隔和通信量情况下的通信带宽、通信延时、带宽抖动和延时抖动方面的性能差异。
 
## 小组思考题

 - （spoc） 每人用python实现[银行家算法](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab7/deadlock/bankers-homework.py)。大致输出可参考[参考输出](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab7/deadlock/example-output.txt)。除了`YOUR CODE`部分需要填写代码外，在算法的具体实现上，效率也不高，可进一步改进执行效率。
 - 
 
>



    30240243X
    Q & A
    Resources
    Statistics

    [王振_计21]
    王振_计21

    Piazza Careers

▾
Note History:
note
54 views
第20讲课堂练习

实现银行家算法

 

问题描述：

https://github.com/xuyongjiande/os_exercises/blob/master/all/08-1-spoc-discussion.md#%E5%B0%8F%E7%BB%84%E6%80%9D%E8%80%83%E9%A2%98

 

请参与课堂讨论的同学回复自己的回答和测试用例。

课堂问答
Updated 1 day ago by Yong XIANG
followup discussions
for lingering questions and comments
Resolved Unresolved
[梁锡豪]
梁锡豪 1 day ago

計22 2012011290 梁錫豪

 

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = []
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j])
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        # YOUR CODE, 2012011290
	
	flag = True;
	for i in range(len(self.need[index])):
		if self.need[index][i] > self.avaliable[i]:
			flag = False
			break;	
        #check END here

        #allocating what they need.
        # YOUR CODE, 2012011290

	if not flag:
		return False

	for i in range(len(self.need[index])):
		self.allocated[index][i] += self.need[index][i];
		self.avaliable[i] -= self.need[index][i]
		self.need[index][i] = 0

	return True

        #allocating END here


    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        # YOUR CODE, 2012011290

	not_finished_count = len(self.max)

	for idx in range(len(self.max)):
		if self.finished[idx] == True:
			not_finished_count -= 1
			continue

		flag = True;
		for i in range(len(self.max[idx])):
			if (self.need[idx][i] > self.avaliable[i]):
				flag = False
				break;
		if flag:
			return True
	
	if not_finished_count == 0:
		return True
	else:
	        return False

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]
# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

# total_resources = [6, 5, 7, 6]
# processes=[]
# b = Bankers(total_resources)
#
# max = [
#     [3, 3, 2, 2],
#     [1, 2, 3, 4],
#     [1, 3, 5, 0],
# ]
# allocated = [
#     [1, 2, 2, 1],
#     [1, 0, 3, 3],
#     [1, 2, 1, 0],
# ]
#
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"
#
#
# total_resources = [10, 10, 8, 5]
# processes=[]
# b = Bankers(total_resources)
# max = [
#         [10, 8, 2,5],
#         [6, 1, 3,1],
#         [3, 1, 4,2],
#         [5, 4, 2,1]
#     ]
# allocated = [
#         [3, 0, 0,3],
#         [1, 1, 2,0],
#         [2, 1, 2,1],
#         [0, 0, 2,0]
#     ]
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"

 
Reply to this followup discussion
Resolved Unresolved
[TTHJ]
TTHJ 1 day ago

计22 黄杰 2012011272

计22 袁源 2012011294

计24 杜鹃 2012011354

https://github.com/THUHJ/os_exercises/blob/master/all/08-1-spoc-discussion.md
Reply to this followup discussion
Resolved Unresolved
[沈光耀]
沈光耀 1 day ago

计22 沈光耀

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = []
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j])
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        # YOUR CODE, YOUR ID
        a = self.need[index]
        for j in range(len(a)):
            if a[j] > self.avaliable[j]:
                return False
        #check END here

        #allocating what they need.
        # YOUR CODE, YOUR ID
        for j in range(len(a)):
            self.allocated[index][j] = self.allocated[index][j]+a[j]
            self.avaliable[j] = self.avaliable[j]-a[j]
            self.need[index][j] = 0
        return True
        #allocating END here

    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        # YOUR CODE, YOU ID
        
        flag = True
        for j in range(len(self.max)):
            if(self.finished[j] == False):
                flag = False

        if flag == True:
            return True

        a = self.need
        flag = True

        for j in range(len(a)):
            if self.finished[j] == False:
                for i in range(len(a[j])):
                    if a[j][i] > self.avaliable[i]:
                        flag = False
                        break
                if flag:
                    return True
        #check END here

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]
# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

# total_resources = [6, 5, 7, 6]
# processes=[]
# b = Bankers(total_resources)
#
# max = [
#     [3, 3, 2, 2],
#     [1, 2, 3, 4],
#     [1, 3, 5, 0],
# ]
# allocated = [
#     [1, 2, 2, 1],
#     [1, 0, 3, 3],
#     [1, 2, 1, 0],
# ]
    #
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"
#
#
# total_resources = [10, 10, 8, 5]
# processes=[]
# b = Bankers(total_resources)
# max = [
#         [10, 8, 2,5],
#         [6, 1, 3,1],
#         [3, 1, 4,2],
#         [5, 4, 2,1]
#     ]
# allocated = [
#         [3, 0, 0,3],
#         [1, 1, 2,0],
#         [2, 1, 2,1],
#         [0, 0, 2,0]
#     ]
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"

Reply to this followup discussion
Resolved Unresolved
[李天润]
李天润 1 day ago

 

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = []
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j])
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        can = True
        for i in range(0, len(self.need[index])):
            if self.need[index][i] > self.avaliable[i]:
                can = False
                break

        if can == False:
            return False

        #allocating what they need.
        for i in range(0, len(self.need[index])):
            self.avaliable[i] -= self.need[index][i]
            self.allocated[index][i] = self.need[index][i]
            self.need[index][i] = 0
        #allocating END here
        return True

    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        can = False
        for i in range(0, len(self.max)):
            can = False
            for j in range(0, len(self.max[i])):
                if self.allocated[i][j] < self.max[i][j]:
                    can = True
                    break
            if can:
                break
        return can

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]
# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

# total_resources = [6, 5, 7, 6]
# processes=[]
# b = Bankers(total_resources)
#
# max = [
#     [3, 3, 2, 2],
#     [1, 2, 3, 4],
#     [1, 3, 5, 0],
# ]
# allocated = [
#     [1, 2, 2, 1],
#     [1, 0, 3, 3],
#     [1, 2, 1, 0],
# ]
#
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"
#
#
# total_resources = [10, 10, 8, 5]
# processes=[]
# b = Bankers(total_resources)
# max = [
#         [10, 8, 2,5],
#         [6, 1, 3,1],
#         [3, 1, 4,2],
#         [5, 4, 2,1]
#     ]
# allocated = [
#         [3, 0, 0,3],
#         [1, 1, 2,0],
#         [2, 1, 2,1],
#         [0, 0, 2,0]
#     ]
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"

 
Reply to this followup discussion
Resolved Unresolved
[wangyan]
wangyan 1 day ago

计24 王妍 2012011352

 

代码请见：

https://github.com/yanwang9411/os_exercises/blob/master/all/banker.py

 
Reply to this followup discussion
Resolved Unresolved
[林子钏]
林子钏 1 day ago

鲁逸沁 2012011314

谢晓晖 2012011315

林子钏 2012011322

史佳欣 2012011299

 

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = [] 
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j]) 
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        # YOUR CODE, YOUR ID
        for i in range(len(self.need[index])):
            if self.need[index][i] > self.avaliable[i]:
                return False
        #check END here

        #allocating what they need.
        # YOUR CODE, YOUR ID
        for i in range(len(self.allocated[index])):
            self.allocated[index][i] += self.need[index][i]
            self.avaliable[i] -= self.need[index][i]
            self.need[index][i] = 0
        return True
        #allocating END here
        #pass

    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        # YOUR CODE, YOU ID
        for i in range(3):
            find = True
            for j in range(4):
                if self.need[i][j] > self.avaliable[j]:
                    find = False
            if find :
                return True
        return False
        #check END here
        #pass

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]
# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

# total_resources = [6, 5, 7, 6]
# processes=[]
# b = Bankers(total_resources)
#
# max = [
#     [3, 3, 2, 2],
#     [1, 2, 3, 4],
#     [1, 3, 5, 0],
# ]
# allocated = [
#     [1, 2, 2, 1],
#     [1, 0, 3, 3],
#     [1, 2, 1, 0],
# ]
#
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"
#
#
# total_resources = [10, 10, 8, 5]
# processes=[]
# b = Bankers(total_resources)
# max = [
#         [10, 8, 2,5],
#         [6, 1, 3,1],
#         [3, 1, 4,2],
#         [5, 4, 2,1]
#     ]
# allocated = [
#         [3, 0, 0,3],
#         [1, 1, 2,0],
#         [2, 1, 2,1],
#         [0, 0, 2,0]
#     ]
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"

 
Reply to this followup discussion
Resolved Unresolved
[qinwei]
qinwei 1 day ago

覃伟 计22 2012011278

 

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = []
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j])
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        # 2012011278
        for i in range(len(self.avaliable)):
            if self.avaliable[i] < self.need[index][i]:
                return False
        #check END here

        #allocating what they need.
        # 2012011278
        for i in range(len(self.avaliable)):
            self.avaliable[i] -= self.need[index][i]
            self.allocated[index][i] += self.need[index][i]
            self.need[index][i] = 0
        return True
        #allocating END here
        # pass

    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        # 2012011278
        qwSafe = True
        qwfinishNum = 0
        for i in range(len(self.max)):
            #print 'qw... finish? %d' % self.finished[i]
            if self.finished[i] == True:
                qwfinishNum += 1
                continue
            qwSafe = True
            for j in range(len(self.avaliable)):
                if self.avaliable[j] < self.need[i][j]:
                    qwSafe = False
                    break
            if qwSafe == True:
                return True
        if qwfinishNum == len(self.max):
            return True
        return False
        #check END here
        # pass

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]

# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

'''
total_resources = [6, 5, 7, 6]
processes=[]
b = Bankers(total_resources)
max = [
     [3, 3, 2, 2],
     [1, 2, 3, 4],
     [1, 3, 5, 0],
]
allocated = [
     [1, 2, 2, 1],
     [1, 0, 3, 3],
     [1, 2, 1, 0],
]

b.SignProcesses(max, allocated)
b.print_matrixes()
result=b.Execute()
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"
'''
'''
total_resources = [10, 10, 8, 5]
processes=[]
b = Bankers(total_resources)
max = [
        [10, 8, 2,5],
        [6, 1, 3,1],
        [3, 1, 4,2],
        [5, 4, 2,1]
    ]
allocated = [
        [3, 0, 0,3],
        [1, 1, 2,0],
        [2, 1, 2,1],
        [0, 0, 2,0]
   ]
b.SignProcesses(max, allocated)
b.print_matrixes()
result=b.Execute()
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"
'''

 
Reply to this followup discussion
Resolved Unresolved
[ZHOU Shengkai]
ZHOU Shengkai 1 day ago

2012011342 周圣凯

2012011392 韦福超

2012011397 王振

2012011401 张梦豪

import os
import random
import numpy as np
import itertools

class Bankers(object):
    def __init__(self, totalResource):
        #initiating
        self.RESOURCE = totalResource

    def SignProcesses(self, max_, allocated_):
        self.max = max_
        self.allocated = allocated_
        self.need = self.CalcNeed()
        self.avaliable = self.CalcAvaliable()
        self.finished = [False]*len(self.allocated)

    def Difference(self,a,b):
        #return matrix subtracted from a by b
        res = []
        for i in range(len(a)):
            tmp = []
            for j in range(len(a[i])):
                tmp.append(a[i][j]-b[i][j])
            res.append(tmp)
        return res

    def CalcNeed(self):
        #calc request by subtracting signed matrix from max matrix
        return self.Difference(self.max,self.allocated)

    def CalcAvaliable(self):
        """Calc Avaliable Resource"""
        a = self.allocated
        res = []
        for j in range(len(a[0])):
            tmp = 0
            for i in range(len(a)):
                tmp += a[i][j]
            res.append(self.RESOURCE[j] - tmp)
        return res

    def ExecuteProcess(self,index):

        #check if less avaliable than Request
        # 2012011342, zsk12
        if any(a<n for a, n in zip(self.avaliable, self.need[index])):
            return False
        #check END here

        #allocating what they need.
        # 2012011342, zsk12
        self.avaliable = [a-n for a, n in zip(self.avaliable, self.need[index])]
        self.need[index] = [0 for n in self.need[index]]
        self.finished[index] = True
        return True
        #allocating END here
        #pass

    def TempSafeCheckAfterRelease(self):
        #check if at least one request can be done after previous process done. not check whole sequances.
        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
        # 2012011342, zsk12
        return any(
                all(a>=n for a, n in zip(self.avaliable, need1))
                for i, need1 in zip(range(len(self.need)), self.need)
                if self.finished[i]
        )
        #check END here
        #pass

    def print_matrixes(self):
        print "_____________________________________________"
        print "MAX\t\tAllocated\tNeed"
        for idx in range(len(self.max)):
            print "%s\t%s\t%s" % (self.max[idx],self.allocated[idx], self.need[idx])
        print "_____________________________________________"
        print "Resources:"
        print "Total: %s\tAvailable: %s\n" % (self.RESOURCE, self.avaliable)

    def ReleasingProcess(self,index):
        for i in range(0,len(self.RESOURCE)):
            self.finished[index] = True
            self.allocated[index][i] = 0
        self.avaliable = self.CalcAvaliable()

    def Execute(self):
        i = 0
        # get all permutation of processes
        perm = itertools.permutations(range(procnum), procnum)
        permArray = np.asarray(list(perm))

        for arr in permArray:
            for i in arr:
                if self.finished[i] == False:
                    print "Executing..."
                    print "Request: "
                    print self.need[i]
                    #check if less avaliable than Request
                    if self.ExecuteProcess(i):
                        print "Dispatching Done..."

                        self.print_matrixes()

                        print "-----Releasing Process------"

                        self.ReleasingProcess(i)

                        self.print_matrixes()

                        #check if at least one request can be done after previous process done. not check whole sequances.
                        #if every element of Requests can't accepted after previous process done, this mean it is not safe state
                        if not (self.TempSafeCheckAfterRelease()):
                            print "SAFE STATE: NOT SAFE - There are no sequances can avoid Deadlock"
                            return False
                        processes.append(i)
                    else:
                        print "HOLD: not enough Resource"

                if i == len(self.allocated)-1:
                    i = 0
                else:
                    i += 1

                check = True
                for k in range(0,len(self.allocated)):
                    if self.finished[k] == False:
                        check = False
                        break
                if check == True:
                    return True
                    break
        #every permutation of processes is false
        return False

def getmax():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain_max=0
            if j >0:
                remain_max=total_resources[i]
                for k in range(j):
                    remain_max=remain_max-res[k][i]
                if remain_max < 0:
                    remain_max=0
            else:
                remain_max=total_resources[i]
            tmp.append((int)(randnum*remain_max*0.8))
        res.append(tmp)
    return res

def getallocated():
    res = []
    for j in range(procnum):
        tmp = []
        for i in range(len(total_resources)):
            randnum=random.random()
            remain=0
            if j >0:
                remain=max[j][i]
                for k in range(j):
                    remain=remain-res[k][i]
                if remain < 0:
                    remain=0
            else:
                remain=max[j][i]
            tmp.append((int)(randnum*remain))
        res.append(tmp)
    return res

print "start here"
# random seed
seed = 2
random.seed(seed)
# the number of process list
procnum = 3
# the number of type of resource
resnum =  4
# the max total value of resource
restotalval = 30
# the total resources list
total_resources=[]
# the total processes
processes=[]
# set the real total value of resource in total_resources
for i in range(resnum):
    total_resources.append((int)(restotalval*random.random()))
# init the Banker
b = Bankers(total_resources)
# get the max request values of resources from process
max=getmax()
# get the already gotted values of resources from process
allocated=getallocated()
# init need matrix, available vector
b.SignProcesses(max, allocated)
# print all theses matrixes
b.print_matrixes()
# executing Banker algorithm
result=b.Execute()
# show results
if result:
    print "SUCCESS proc lists ",processes
else:
    print "Failed"

# total_resources = [6, 5, 7, 6]
# processes=[]
# b = Bankers(total_resources)
#
# max = [
#     [3, 3, 2, 2],
#     [1, 2, 3, 4],
#     [1, 3, 5, 0],
# ]
# allocated = [
#     [1, 2, 2, 1],
#     [1, 0, 3, 3],
#     [1, 2, 1, 0],
# ]
#
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"
#
#
# total_resources = [10, 10, 8, 5]
# processes=[]
# b = Bankers(total_resources)
# max = [
#         [10, 8, 2,5],
#         [6, 1, 3,1],
#         [3, 1, 4,2],
#         [5, 4, 2,1]
#     ]
# allocated = [
#         [3, 0, 0,3],
#         [1, 1, 2,0],
#         [2, 1, 2,1],
#         [0, 0, 2,0]
#     ]
# b.SignProcesses(max, allocated)
# b.print_matrixes()
# result=b.Execute()
# if result:
#     print "SUCCESS proc lists ",processes
# else:
#     print "Failed"

 - (spoc) 以小组为单位，请思考在lab1~lab5的基础上，是否能够实现IPC机制，请写出如何实现信号，管道或共享内存（三选一）的设计方案。
 
 - (spoc) 扩展：用C语言实现某daemon程序，可检测某网络服务失效或崩溃，并用信号量机制通知重启网络服务。[信号机制的例子](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab7/ipc/signal-ex1.c)

 - (spoc) 扩展：用C语言写测试用例，测试管道、消息队列和共享内存三种通信机制进行不同通信间隔和通信量情况下的通信带宽、通信延时、带宽抖动和延时抖动方面的性能差异。[管道的例子](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab7/ipc/pipe-ex2.c)
