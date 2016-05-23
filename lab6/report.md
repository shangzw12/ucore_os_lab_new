### Lab6

### ��ϰ0����д����ʵ��

	�����

### ��ϰ1��ʹ�� Round Robin �����㷨

####��ϰ1.1:����Ⲣ����sched_calss�и�������ָ����÷������Ӻ�Round Robin �����㷨��ucore�ĵ���ִ�й���

	�Ӵ����Ϸ�������Ҫ���⼸��ָ�룺

```
struct sched_class default_sched_class = {
    .name = "stride_scheduler",
    .init = stride_init,
    .enqueue = stride_enqueue,
    .dequeue = stride_dequeue,
    .pick_next = stride_pick_next,
    .proc_tick = stride_proc_tick,
};
```

	������Щ�Ǻ���ָ�룺

```
    .init = stride_init,
    .enqueue = stride_enqueue,
    .dequeue = stride_dequeue,
    .pick_next = stride_pick_next,
    .proc_tick = stride_proc_tick,
```

	init�ڳ���ʼִ�У����н��̴���֮ǰ�����á�
	enqueue�ڽ��̼����������ʱ�����á�
	dequeue�ڽ��̱��Ƴ���������ʱ�����á�
	pick_next�ڽ����л�ʱ�����ã������ķ���ֵ��Ϊ�л����Ľ��̡�
	proc_tick�ڲ���ʱ���ж�ʱ�����á�

####��ϰ1.2: ����ʵ�鱨���м�Ҫ˵��������ʵ�֡��༶�������е����㷨����������Ҫ��ƣ�����������ϸ���

	��Ҫ��ƣ���һ�����е��б����Ƕ��е��ܵ������������������ĸ�������pick_next��Ϊ�л����Ľ��̡������ж�����У����Ĺ��ܿ��ܾ���ʵ��һ����Round Robin�����㷨����Stride Scheduling�����㷨�������ܵ���������һ�����̰����ĸ������ϣ���ô��������̵�`enqueue`��`dequeue`������Ӧ�Ķ���ִ�С�����`proc_tick`ʱ��Ӧ�õ�����һ��ѡ���л����̵Ķ��е�`proc_tick`��

####��ϰ2: ʵ�� Stride Scheduling �����㷨

��ϰ2.0 ����ʵ�鱨���м�Ҫ˵��������ʵ�ֹ���

	���ݺ겻ͬ`USE_SKEW_HEAP`��Ϊ�����㷨�����˲�ͬ����Ϊ����Ϊ1ʱʹ����ʽ�ѣ�б�ѣ���Ч�ʿ��ܸ��ߡ�
	
	��ʼ���������ڽ�������Ϊ0�����ݺ��ʼ���ѻ�����

```
#if USE_SKEW_HEAP
    rq->lab6_run_pool = 0;
#else
    list_init(&rq->run_list);
#endif
    rq->proc_num = 0;
```

> ��ӣ�����ѻ��������Ϊ���̷���ʱ��Ƭ�����µ��ȶ����н��̵�����������������ȼ�δ����ʼ�������ڼ������ʱ��ʼ������

```
#if USE_SKEW_HEAP
    rq->lab6_run_pool = skew_heap_insert(rq->lab6_run_pool, &(proc->lab6_run_pool), proc_stride_comp_f);
#else
    list_add(&rq->run_list, &proc->run_link);
#endif
    proc->time_slice = rq->max_time_slice;
    proc->rq = rq;
    if (proc->lab6_priority == 0) proc->lab6_priority = 1;
    rq->proc_num++;
```

> ���ӣ��Ӷ���ɾ�����������ɾ�������µ��ȶ����н��̵���������

```
#if USE_SKEW_HEAP
    rq->lab6_run_pool = skew_heap_remove(rq->lab6_run_pool, &(proc->lab6_run_pool), proc_stride_comp_f);
#else
    list_del(&proc->run_link);
#endif
    rq->proc_num--;
```

> ѡȡ�����ʹ����ʽ�ѣ�ѡȡ�Ѷ�Ԫ�ؼ��ɣ����򣬱��������е�Ԫ�أ��ҳ�lab6_stride��С�Ľ��̡�

```
#if USE_SKEW_HEAP
    if (!rq->lab6_run_pool) return NULL;
    struct proc_struct *p = le2proc(rq->lab6_run_pool, lab6_run_pool);
#else
    list_entry_t *le;
    struct proc_struct *p = 0;
    for (le = list_next(&rq->run_list); le != &rq->run_list; le = list_next(le)) {
        struct proc_struct *q = le2proc(le, run_link);
        if (!p || proc_stride_comp_f(&p->lab6_run_pool, &q->lab6_run_pool) == 1) p = q;
    }
#endif
    if (p) p->lab6_stride += BIG_STRIDE / p->lab6_priority;
    return p;
```

> ʱ���жϣ�ʱ��Ƭ��һ�����ʱ��Ƭ�þ������µ���֮��

```
    if (--proc->time_slice <= 0) proc->need_resched = 1;
```

### ��Ҫ��֪ʶ��

	Round Robin�����㷨��
	
	Stride Scheduling�����㷨��
	
	�༶�������е����㷨��
