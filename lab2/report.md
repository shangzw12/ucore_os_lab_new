##lab2实验报告

####练习0：填写已有实验
	代码中填写完毕，不过本次实验中用不到之前的代码。
####练习1：实现first-fit连续物理内存分配算法
	本练习涉及到的函数有default_init, default_init_memmap,default_alloc_pages, default_free_pages
	但是default_init函数并不需要修改。
	此外其他函数的实现思路：
		default_init_memmap:本函数用于给一个起始地址为base的含有n个页的块
		建立first-fit所需要的映射关系，并将内存连接到free_list中，同时设置相关的标志位。
		default_alloc_pages：本函数是first-fit算法的真正实现部分，从尾部开始搜索free_list
		直到找到第一个比需要的内存大的的块，然后将原来的块划分为一块或两块用作分配，
		之后修改块中剩余的页的起始页中对应的各种标志信息。
		default_free_pages: 本函数是first-fit算法中又一个重要部分，用于将释放掉的
		内存重新组织到free_list中，此时需要注意空闲块的合并。真正实现中并不复杂，只需要
		对于释放掉的块的相邻内存块进行搜索+合并即可完成。
####练习2：实现寻找虚拟地址对应的页表项
	本题只涉及到get_pte函数
#####[练习2.1] 请描述页目录项（Pag Director Entry）和页表（Page Table Entry）中每个组成部分的含义和以及对ucore而言的潜在用处。
	PDE：页目录项，里面存放着二级页表项的起始地址和三个标志位——有效、可写和可被用户访问
	PTE：页表项，里面存放着逻辑地址对应的物理地址页的起始地址和两个标志位。——有效和可写
	各标志位含义及说明在mmu.h中：
	#define PTE_P           0x001                   // Present
	#define PTE_W           0x002                   // Writeable
	#define PTE_U           0x004                   // User
	#define PTE_PWT         0x008                   // Write-Through
	#define PTE_PCD         0x010                   // Cache-Disable
	#define PTE_A           0x020                   // Accessed
	#define PTE_D           0x040                   // Dirty
	#define PTE_PS          0x080                   // Page Size
	#define PTE_MBZ         0x180                   // Bits must be zero
	#define PTE_AVAIL       0xE00                   // Available for software use
                                                // The PTE_AVAIL bits aren't used by the kernel or interpreted by the
                                               // hardware, so user processes are allowed to set them arbitrarily.
#####[练习2.2] 如果ucore执行过程中访问内存，出现了页访问异常，请问硬件要做哪些事情？
	触发缺页异常，从GDT中找到页访问异常服务程序的入口，然后交由OS处理

####练习3、释放某虚地址所在的页并取消对应二级页表项的映射
#####[练习3.1] 数据结构Page的全局变量（其实是一个数组）的每一项与页表中的页目录项和页表项有无对应关系？如果有，其对应关系是啥？
	有对应关系，页表是从end开始的一段连续的内存
#####[练习3.2] 如果希望虚拟地址与物理地址相等，则需要如何修改lab2，完成此事？
	在页分配算法中，alloc_page加入一个参数，即指定要分配的空间的起始地址。
	不过这个并不一定总能实现，如果找不到满足条件的内存页或者页被分配出去，则不能完成目标。

####与参考答案的区别：
	除了多出很多注释之外，并无本质区别。
####重要知识点
	first-fit算法。
	done！
	
	
	