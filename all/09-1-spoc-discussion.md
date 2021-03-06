# 文件系统(lec 21) spoc 思考题

## 个人思考题
### 文件系统和文件 
 1. 文件系统的功能是什么？

>  负责数据持久保存，功能是数据存储和访问

>  具体功能：文件分配、文件管理、数据可靠和安全

 1. 什么是文件？

>  文件系统中具有符号名的基本数据单位。

### 文件描述符
 1. 打开文件时，文件系统要维护哪些信息？

>  文件指针、打开文件计数、访问权限、文件位置和数据缓存

 1. 文件系统的基本数据访问单位是什么？这对文件系统有什么影响？
 1. 文件的索引访问有什么特点？如何优化索引访问的速度？

### 目录、文件别名和文件系统种类
 1. 什么是目录？

>  由文件索引项组成的特殊文件。

 1. 目录的组织结构是什么样的？

>  树结构、有向图

 1. 目录操作有哪些种类？
 1. 什么是文件别名？软链接和硬链接有什么区别？
 1. 路径遍历的流程是什么样的？如何优化路径遍历？
 1. 什么是文件挂载？
 1. 为什么会存在大量的文件类型？

### 虚拟文件系统 
 1. 虚拟文件系统的功能是什么？

>  对上对下的接口、高效访问实现

 1. 文件卷控制块、文件控制块和目录项的相互关系是什么？
 1. 可以把文件控制块放到目录项中吗？这样做有什么优缺点？


### 文件缓存和打开文件
 1. 文件缓存和页缓存有什么区别和联系？
 1. 为什么要同时维护进程的打开文件表和操作系统的打开文件表？这两个打开文件表有什么区别和联系？为什么没有线程的打开文件表？
 
### 文件分配
 1. 文件分配的三种方式是如何组织文件数据块的？各有什么特征？
 1. UFS多级索引分配是如何组织文件数据块的位置信息的？

### 空闲空间管理和冗余磁盘阵列RAID
 1. 硬盘空闲空间组织和文件分配有什么异同？
 1. RAID-0、1、4和5分别是如何组织磁盘数据块的？各有什么特征？

## 小组思考题
 1. (spoc)完成Simple File System的功能，支持应用程序的一般文件操作。具体帮助和要求信息请看[sfs-homework](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab8/sfs-homework.md)
    ARG seed 0
    ARG numInodes 8
    ARG numData 8
    ARG numRequests 10
    ARG reverse False
    ARG printFinal False

    Initial state

    inode bitmap  10000000
    inodes        [d a:0 r:2] [] [] [] [] [] [] [] 
    data bitmap   10000000
    data          [(.,0) (..,0)] [] [] [] [] [] [] [] 

    Which operation took place?

    > mkdir /g

    inode bitmap  11000000
    inodes        [d a:0 r:3] [d a:1 r:2] [] [] [] [] [] [] 
    data bitmap   11000000
    data          [(.,0) (..,0) (g,1)] [(.,1) (..,0)] [] [] [] [] [] [] 

    Which operation took place?

    > creat /q

    inode bitmap  11100000
    inodes        [d a:0 r:4] [d a:1 r:2] [f a:-1 r:1] [] [] [] [] [] 
    data bitmap   11000000
    data          [(.,0) (..,0) (g,1) (q,2)] [(.,1) (..,0)] [] [] [] [] [] [] 

    Which operation took place?

    > creat /u

    inode bitmap  11110000
    inodes        [d a:0 r:5] [d a:1 r:2] [f a:-1 r:1] [f a:-1 r:1] [] [] [] [] 
    data bitmap   11000000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3)] [(.,1) (..,0)] [] [] [] [] [] [] 

    Which operation took place?

    > link /u /x

    inode bitmap  11110000
    inodes        [d a:0 r:6] [d a:1 r:2] [f a:-1 r:1] [f a:-1 r:2] [] [] [] [] 
    data bitmap   11000000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (x,3)] [(.,1) (..,0)] [] [] [] [] [] [] 

    Which operation took place?

    > mkdir /t

    inode bitmap  11111000
    inodes        [d a:0 r:7] [d a:1 r:2] [f a:-1 r:1] [f a:-1 r:2] [d a:2 r:2] [] [] [] 
    data bitmap   11100000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (x,3) (t,4)] [(.,1) (..,0)] [(.,4) (..,0)] [] [] [] [] [] 

    Which operation took place?

    > creat /g/c

    inode bitmap  11111100
    inodes        [d a:0 r:7] [d a:1 r:3] [f a:-1 r:1] [f a:-1 r:2] [d a:2 r:2] [f a:-1 r:1] [] [] 
    data bitmap   11100000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (x,3) (t,4)] [(.,1) (..,0) (c,5)] [(.,4) (..,0)] [] [] [] [] [] 

    Which operation took place?

    > unlink /x

    inode bitmap  11111100
    inodes        [d a:0 r:6] [d a:1 r:3] [f a:-1 r:1] [f a:-1 r:1] [d a:2 r:2] [f a:-1 r:1] [] [] 
    data bitmap   11100000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (t,4)] [(.,1) (..,0) (c,5)] [(.,4) (..,0)] [] [] [] [] [] 

    Which operation took place?

    > mkdir /g/w

    inode bitmap  11111110
    inodes        [d a:0 r:6] [d a:1 r:4] [f a:-1 r:1] [f a:-1 r:1] [d a:2 r:2] [f a:-1 r:1] [d a:3 r:2] [] 
    data bitmap   11110000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (t,4)] [(.,1) (..,0) (c,5) (w,6)] [(.,4) (..,0)] [(.,6) (..,1)] [] [] [] [] 

    Which operation took place?

    > write /g/c 'o'

    inode bitmap  11111110
    inodes        [d a:0 r:6] [d a:1 r:4] [f a:-1 r:1] [f a:-1 r:1] [d a:2 r:2] [f a:4 r:1] [d a:3 r:2] [] 
    data bitmap   11111000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (t,4)] [(.,1) (..,0) (c,5) (w,6)] [(.,4) (..,0)] [(.,6) (..,1)] [o] [] [] [] 

    Which operation took place?

    > creat /n

    inode bitmap  11111111
    inodes        [d a:0 r:7] [d a:1 r:4] [f a:-1 r:1] [f a:-1 r:1] [d a:2 r:2] [f a:4 r:1] [d a:3 r:2] [f a:-1 r:1] 
    data bitmap   11111000
    data          [(.,0) (..,0) (g,1) (q,2) (u,3) (t,4) (n,7)] [(.,1) (..,0) (c,5) (w,6)] [(.,4) (..,0)] [(.,6) (..,1)] [o] [] [] [] 
```
code:
```
def deleteFile(self, tfile):
        if printOps:
            print 'unlink("%s");' % tfile

        inum = self.nameToInum[tfile]
        
        # IF inode.refcnt ==1, THEN free data blocks first, then free inode, ELSE dec indoe.refcnt
        # remove from parent directory: delete from parent inum, delete from parent addr

        finode = self.inodes[inum]
        finode.decRefCnt()
        if finode.getRefCnt()<=0:
            self.dataFree(finode.addr)
            self.inodeFree(inum)

        parent = self.getParent(tfile)
        pinum = self.nameToInum[parent]
        pinode = self.inodes[pinum]
        pblock = self.data[pinode.addr]

        basename = tfile.split('/')[-1]
        pblock.delDirEntry(basename)

        # finally, remove from files list
        self.files.remove(tfile)
        return 0

    def createLink(self, target, newfile, parent):
   
        # find info about parent
        # is there room in the parent directory?
        # if the newfile was already in parent dir?
        # now, find inumber of target
        # inc parent ref count
        # now add to directory

        # get parent info
        #print '#link#', target, newfile, parent
        pinum = self.nameToInum[parent]
        pinode = self.inodes[pinum]
        pblock = self.data[pinode.addr]
        if not pblock.getFreeEntries()>0:
            return -1
        if pblock.dirEntryExists(newfile):
            return -1

        # get target
        tinum = self.nameToInum[target]
        tinode = self.inodes[tinum]

        # add to directory
        pblock.addDirEntry(newfile, tinum)
        tinode.incRefCnt()
        return tinum

    def createFile(self, parent, newfile, ftype):
    
        # find info about parent
        # is there room in the parent directory?
        # have to make sure file name is unique
        # find free inode
        # if a directory, have to allocate directory block for basic (., ..) info
        # now ok to init inode properly
        # inc parent ref count
        # and add to directory of parent
  
        #print '#create#', parent, newfile, ftype
        # get parent info
        pinum = self.nameToInum[parent]
        pinode = self.inodes[pinum]
        pblock = self.data[pinode.addr]

        if not pblock.getFreeEntries()>0:
            return -1
        if pblock.dirEntryExists(newfile):
            return -1

        # alloc
        finum = self.inodeAlloc()
        if finum<0: return -1
        finode = self.inodes[finum]
        pblock.addDirEntry(newfile, finum)
        finode.setAll(ftype, -1, 1)

        # directory
        if ftype=='d':
            finode.addr = self.dataAlloc()
            if finode.addr<0:
                pblock.delDirEntry(newfile)
                self.inodeFree(finum)
                return -1
            fblock = self.data[finode.addr]
            fblock.setType('d')
            fblock.addDirEntry('.', finum)
            finode.incRefCnt()
            fblock.addDirEntry('..', pinum)
            pinode.incRefCnt()

        return finum

    def writeFile(self, tfile, data):
        inum = self.nameToInum[tfile]
        curSize = self.inodes[inum].getSize()
        dprint('writeFile: inum:%d cursize:%d refcnt:%d' % (inum, curSize, self.inodes[inum].getRefCnt()))

   
        # file is full?
        # no data blocks left
        # write file data
   
        dprint('#write# {} {}'.format(tfile, data))
        if curSize>0:
            return -1
        finode = self.inodes[inum]

        # create block
        if finode.addr==-1:
            finode.addr = self.dataAlloc()
            dprint('## {}'.format(finode.addr))
            if finode.addr<0: return -1
            self.data[finode.addr].setType('f')

        fblock = self.data[finode.addr]
        fblock.addData(data)

        if printOps:
            print 'fd=open("%s", O_WRONLY|O_APPEND); write(fd, buf, BLOCKSIZE); close(fd);' % tfile
        return 0
```

 1. (spoc)FAT、UFS、YAFFS、NTFS这几种文件系统中选一种，分析它的文件卷结构、目录结构、文件分配方式，以及它的变种。
  wikipedia上的文件系统列表参考
  - http://en.wikipedia.org/wiki/List_of_file_systems
  - http://en.wikipedia.org/wiki/File_system
  - http://en.wikipedia.org/wiki/List_of_file_systems

  请同学们依据自己的选择，在下面链接处回复分析结果。
  - [https://piazza.com/class/i5j09fnsl7k5x0?cid=416 FAT文件系统分析]
  - [https://piazza.com/class/i5j09fnsl7k5x0?cid=417 NTFS文件系统分析]
  - [https://piazza.com/class/i5j09fnsl7k5x0?cid=418 UFS文件系统分析]
  - [https://piazza.com/class/i5j09fnsl7k5x0?cid=419 ZFS文件系统分析]
  - [https://piazza.com/class/i5j09fnsl7k5x0?cid=420 YAFFS文件系统分析]
