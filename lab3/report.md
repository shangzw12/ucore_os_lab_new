##Lab3ʵ�鱨��
###��ϰ0����д����ʵ��
	done��
###��ϰ1����δ��ӳ��ĵ�ַӳ��������ҳ
####������ҳĿ¼�Pag Director Entry����ҳ����Page Table Entry������ɲ��ֶ�ucoreʵ��ҳ�滻�㷨��Ǳ���ô���
	�ô��Ƕ�λҳ�棬���˵��Ƿ����ʵ�ҳ�档
####���ucore��ȱҳ����������ִ�й����з����ڴ棬������ҳ�����쳣������Ӳ��Ҫ����Щ���飿
	�ٴε���ȱҳ�������̣������ϴγ����쳣��ҳ�档
###��ϰ2��������ɻ���FIFO��ҳ���滻�㷨
####���Ҫ��ucore��ʵ��"extended clockҳ�滻�㷨"��������Ʒ��������е�swap_manager����Ƿ�����֧����ucore��ʵ�ִ��㷨������ǣ���������Ʒ�����������ǣ����������µ���չ�ͻ�����չ����Ʒ���������Ҫ�ش��������⣺
	�����ڵĿ��ֻ��map_swappable��set_unswappable��swap_out_victim����������û�ж�Ӧ��ҳ�����ʱ�����ĺ��������ҳ���滻�㷨�޷���֪ҳ���Ƿ񱶷��ʹ�����չ����������һ����Ϊ����ҳ�汻���ʣ�����map_modified���ĺ�����Լ����ҳ�汻����ʱ���á�
#####��Ҫ��������ҳ��������ʲô��
	�޸�λ�ͷ���λ����0�������ǡ�ʱ��ָ�롱ɨ���ĵ�һ��������ҳ��
#####��ucore������жϾ�������������ҳ��
	�޸�λ��������ҳ���PTE_D��ǡ�����λ�͡�ʱ��ָ�롱��ҳ���û��㷨���м�¼�����з���λ������������������Ǹ�������
#####��ʱ���л���ͻ���������
	ҳȱʧʱ���벢������
###��Ҫ��֪ʶ��
	FIFO PRA��

	��Ӧ֪ʶ�㣺�ֲ�ҳ���滻FIFO�㷨��

	���壺����Ҫ��ҳ�滻��ʱ���������Ȼ����ҳ��

	��ϵ����������ͬһ���㷨��

	���죺OSԭ���У�ҳ�滻��ͻ�����ͬʱ���еģ���һ���������������ԣ�ҳ����ʱ������һ��ҳ��һ���ỻ��һ��ҳ�棬��֤ҳ���������䣩��ʵ���У�ҳ�滻������_fifo_map_swappable��ʵ�ֵģ���������_fifo_swap_out_victim��ʵ�ֵģ�������������FIFO�㷨���뱾�����ܱ�֤���������������ԣ�Ҫ�ɵ���������֤�����ԡ�