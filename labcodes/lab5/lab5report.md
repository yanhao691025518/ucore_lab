##lab5 report
#####2012011330 闫昊
***
###练习一 加载应用程序并执行
> 实验设计
1. 设置trapframe中的段为用户段
2. 设置esp、eip
3. tf_eflags为FL_IF，运行中断

> 用户态进程被ucore选择占用CPU执行（RUNNING态）到具体执行应用程序第一条指令的整个经过。
* 第一个用户态程序通过kernel调用do_fork从init_main开始执行
* 当do_fork创建新进程时，它被插入进程队列，schedule选择它执行，最后实际上是通过switch_to落实的
* proc_run进行了用户程序运行环境的设定，最后调用switch_to，通过pushl和ret切换了进程
###练习二 父进程复制自己的内存空间给子进程
> copy on right
1. 具体实现思路可以是将父进程和子进程对于他们共享的地址空间的访问设置为只读（通过页表）
2. 某一方Write的时候，会触发Page Fault，这时对该页进行拷贝，并且设置双方可写，修改某一方的映射为新的页即可
3. 之后双方对于该页可以正常读写

###练习三 阅读分析源代码，理解进程执行 fork/exec/wait/exit 的实现，以及系统调用的实现
* fork:
	分配空间
    设kstack
    设memory manager
    复制tf和context
    加入进程列表
    
* exec:
	调用exit_mmap(mm)&pug_pgdir(mm)
    调用load_icode新的memory space

* wait:
	如果发现子进程有处于僵尸态的，释放退出子进程
    如果有子进程但没处于僵尸态的，则当前进程进入sleeping
    
* exit
	释放内存
	把自己的状态设为僵尸态
    唤醒父进程
    把自己的子进程过给init_proc进程
    
*fork是创建进程  exec载入进程   wait等待子进程退出   exit退出

>fork,exec 创建进程
run RUNNABLE -> RUNNING
wait RUNNING -> WAIT
wakeup WAIT -> RUNNABLE
exit RUNNING -> ZOMBIE