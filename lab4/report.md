#Lab4实验报告
##练习0：填写已有实验
	Done！
	
##练习1：分配并初始化一个进程控制块

####练习1.1：请在实验报告中简要说明你的设计实现过程
	首先通过调用kmallco函数获得proc_struct结构的一块内存块，作为第0个进程控制块。
	并把proc中的各成员变量清零，但是需要把有些成员变量设置成特殊的值。：
	
	 proc->state = PROC_UNINIT; //设置进程为初始态
     proc->pid = -1;//没有分配进程id之前，默认设置为-1
     proc->cr3 = boot_cr3;  //使用内核页目录表的基址，因为是内核线程
	 
####练习1.1 请说明proc_struct中struct context context和struct trapframe *tf成员变量含义和在本实验中的作用是啥？（提示通过看代码和编程调试可以判断出来）
	context用存储进程上下文信息的结构体，在线程切换的时候保留元进程的上下文到该结构体
	并且从该结构体中恢复切换到的进程的上下文；
	tf是指向中断帧结构体的指针
	
##练习2：为新创建的内核线程分配资源

##练习2.1：请在实验报告中简要说明你的设计实现过程

>分为这几步实现：

>1. 分配并初始化进程控制块（alloc_proc函数）；
>2. 分配并初始化内核栈（setup_stack函数）；
>3. 根据clone_flag标志复制或共享进程内存管理结构（copy_mm函数）；
>4. 设置进程在内核（将来也包括用户态）正常运行和调度所需的中断帧和执行上下文（copy_thread函数）；
>5. 把设置好的进程控制块放入hash_list和proc_list两个全局进程链表中；但是此前需要先给进程分配ID
>6. 自此，进程已经准备好执行了，把进程状态设置为“就绪”态；
>7. 设置返回码为子进程的id号。

>如果上述前3步执行没有成功，则需要做对应的出错处理，把相关已经占有的内存释放掉。在第5步里，为了防止多线程不同步问题，需要暂时关中断。

####练习2.1 请说明ucore是否做到给每个新fork的线程一个唯一的id？请说明你的分析和理由。
	
	是！get_pid函数负责分配PID，此函数循环查找了proc_list链表，一旦将要分配的PID与链表中某个进程的PID相同
	就把将要分配的PID加1。函数保证区间[last_pid, next_safe)中没有已经被分配的PID，一旦这个区间
	长度缩小为0，就重新遍历proc_list链表，最终保证PID没有重复。

##练习3 阅读代码，理解 proc_run 函数和它调用的函数如何完成进程切换的
	
####练习3.1 请在实验报告中简要说明你对proc_run函数的分析
	proc_run 在关中断的情况下，进行如下操作
	current = proc;
    load_esp0(next->kstack + KSTACKSIZE);
    lcr3(next->cr3);
    switch_to(&(prev->context), &(next->context));
	load_esp0和lcr3加载新进程的esp和cr3，切换内核栈、二级页表。
	switch_to切换进程上下文，包括寄存器、用户栈等，并把指令指针准备在栈顶ret读取返回值的位置。

####练习3.2 在本实验的执行过程中，创建且运行了几个内核线程？
	2个

####练习3.3 语句local_intr_save(intr_flag);....local_intr_restore(intr_flag);在这里有何作用?请说明理由
	暂时关闭中断：如果当前中断为开，则在local_intr_save关闭中断，待local_intr_restore再打开中断；
	如果当前中断为关，那么什么也不做。
	目的是防止在进程切换的时候被中断打断，造成一些寄存器的值改变，导致恢复进程上下文或恢复页表时发生错误。

##练习3： 重要知识点
	fork
	对应知识点：fork
	含义：复制一个几乎一样的进程，它们各自拥有独立的用户内存空间。
		在父进程中返回子进程ID，在子进程中返回0。如果出现错误，返回负值。
	差异：OS原理中，主要讲fork产生的行为、结果。
		实验中，主要需要知道fork怎么实现，具体需要复制哪些成员，需要修改哪些成员。
	