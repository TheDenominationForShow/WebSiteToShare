>Create by jzf@2018/12/19


# windows下的调试工具

    起源于测试说我这边内存泄露，然后一直没有定位出来什么原因，他使用的资源监视器进行日志打印发现的，但是我的代码没有发现问题啊，就开始怀疑到底怎么回事。然后布了内存泄漏的检测工具，我们部门同事
    利用umdh写的一个bat。
    截出来的内存快照我看不懂啊都是些神马啊
    都是这个样式的：
        + 5320 ( f110 - 9df0) 3a allocs BackTrace00B53 
    后来我和我们leader在那说我感觉这东西没用啊，他是不是检测的我申请了多少次内存啊，和泄漏挂不上钩，我们领导让我自己写一个程序测试下到底什么情况。我就测试了一下发现自己理解的有问题，继续深思，
    发现这东西还是非常有用的啊。
    去MS学习一下去，又提高了不少的认识

    使用gflags+umdh分析内存分析真的有用啊。
    
    目前的理解是gflags保存快照，umdh生成比对文件

    它会将每次申请内存的地方以 BackTraceXXX的形式记录
    使用命令生成比对结果
    + 5320 ( f110 - 9df0) 3a allocs BackTrace00B53 
    00005320 bytes in 0x14 allocations (@ 0x00000428) by: BackTrace00B53
    ntdll!RtlDebugAllocateHeap+0x000000FD
    ntdll!RtlAllocateHeapSlowly+0x0000005A
    ntdll!RtlAllocateHeap+0x00000808
    MyApp!_heap_alloc_base+0x00000069
    MyApp!_heap_alloc_dbg+0x000001A2
    MyApp!_nh_malloc_dbg+0x00000023
    MyApp!_nh_malloc+0x00000016
    MyApp!operator new+0x0000000E
    MyApp!DisplayMyGraphics+0x0000001E
    MyApp!main+0x0000002C
    MyApp!mainCRTStartup+0x000000FC
    KERNEL32!BaseProcessStart+0x0000003D 

    分别是增加了多少字节(16进制？？，我测试的时候感觉不是16进制啊) （快照b的申请 - 快照a的申请） 分配了多少次  记录点名称
    调用堆栈

 ## 地址
 >内存泄露检测的目录 https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/using-umdh-to-find-a-user-mode-memory-leak
 >大目录 https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/
