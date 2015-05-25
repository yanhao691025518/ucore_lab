##lab6 report
#####2012011330 闫昊
***
###练习一 使用 Round Robin 调度算法
> 理解并分析sched_calss中各个函数指针的用法，并接合Round Robin 调度算法描ucore的调度执行过程
* init：初始化列表
* enqueue，将进程放入队列
* dequeue，将进程出队
* pick_next选择下一个要执行的进程
 proc_tick处理时钟中断

***
* 每个时钟tick，run_timer_list被调用，proc_tick会更新进程的时间片，唤醒（enqueue）sleep时间满足的进程
* 每100个tick，会设置当前进程need_resched
* 每个trap，如果是在用户态被打断，如果need_resched，就调用schedule重新调度
* schedule中会enqueue当前进程，pick以后dequeue新的要运行的进程。



> 实现多级反馈队列调度算法
* 设置多个队列，每个队列的时间片随优先级降低而增加
* 每次从优先级最高的队列中选取进程执行，若最高级队列为空队列，则选次优先级队列，以此类推

###练习二 实现 Stride Scheduling 调度算法
> 实验设计

* 总体维护一个优先队列（这里用左偏树实现）
* init:初始化队列
* enqueue:插入一个进程，设置时间片
* dequeue:从队列中，删除一个进程
* pick_next:取队列的第一个，执行
* proc_tick：递减时间片，若剩下时间片为0，则将need_reched设为1，进行调度