##lab2ʵ�鱨��

####��ϰ0����д����ʵ��
	��������д��ϣ���������ʵ�����ò���֮ǰ�Ĵ��롣
####��ϰ1��ʵ��first-fit���������ڴ�����㷨
	����ϰ�漰���ĺ�����default_init, default_init_memmap,default_alloc_pages, default_free_pages
	����default_init����������Ҫ�޸ġ�
	��������������ʵ��˼·��
		default_init_memmap:���������ڸ�һ����ʼ��ַΪbase�ĺ���n��ҳ�Ŀ�
		����first-fit����Ҫ��ӳ���ϵ�������ڴ����ӵ�free_list�У�ͬʱ������صı�־λ��
		default_alloc_pages����������first-fit�㷨������ʵ�ֲ��֣���β����ʼ����free_list
		ֱ���ҵ���һ������Ҫ���ڴ��ĵĿ飬Ȼ��ԭ���Ŀ黮��Ϊһ��������������䣬
		֮���޸Ŀ���ʣ���ҳ����ʼҳ�ж�Ӧ�ĸ��ֱ�־��Ϣ��
		default_free_pages: ��������first-fit�㷨����һ����Ҫ���֣����ڽ��ͷŵ���
		�ڴ�������֯��free_list�У���ʱ��Ҫע����п�ĺϲ�������ʵ���в������ӣ�ֻ��Ҫ
		�����ͷŵ��Ŀ�������ڴ���������+�ϲ�������ɡ�
####��ϰ2��ʵ��Ѱ�������ַ��Ӧ��ҳ����
	����ֻ�漰��get_pte����
#####[��ϰ2.1] ������ҳĿ¼�Pag Director Entry����ҳ��Page Table Entry����ÿ����ɲ��ֵĺ�����Լ���ucore���Ե�Ǳ���ô���
	PDE��ҳĿ¼��������Ŷ���ҳ�������ʼ��ַ��������־λ������Ч����д�Ϳɱ��û�����
	PTE��ҳ������������߼���ַ��Ӧ�������ַҳ����ʼ��ַ��������־λ��������Ч�Ϳ�д
	����־λ���弰˵����mmu.h�У�
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
#####[��ϰ2.2] ���ucoreִ�й����з����ڴ棬������ҳ�����쳣������Ӳ��Ҫ����Щ���飿
	����ȱҳ�쳣����GDT���ҵ�ҳ�����쳣����������ڣ�Ȼ����OS����

####��ϰ3���ͷ�ĳ���ַ���ڵ�ҳ��ȡ����Ӧ����ҳ�����ӳ��
#####[��ϰ3.1] ���ݽṹPage��ȫ�ֱ�������ʵ��һ�����飩��ÿһ����ҳ���е�ҳĿ¼���ҳ�������޶�Ӧ��ϵ������У����Ӧ��ϵ��ɶ��
	�ж�Ӧ��ϵ��ҳ���Ǵ�end��ʼ��һ���������ڴ�
#####[��ϰ3.2] ���ϣ�������ַ�������ַ��ȣ�����Ҫ����޸�lab2����ɴ��£�
	��ҳ�����㷨�У�alloc_page����һ����������ָ��Ҫ����Ŀռ����ʼ��ַ��
	�����������һ������ʵ�֣�����Ҳ��������������ڴ�ҳ����ҳ�������ȥ���������Ŀ�ꡣ

####��ο��𰸵�����
	���˶���ܶ�ע��֮�⣬���ޱ�������
####��Ҫ֪ʶ��
	first-fit�㷨��
	done��
	
	
	