##lab8 report
#####2012011330 闫昊
***
###练习一 完成读文件操作的实现
> UNIX的PIPE机制

* open 检查是否仅有可读或者仅有可写
* close 什么都不做
* write 如果要写的话，就写到一个缓存里，并且唤醒等待读取的进程
* read 从缓存里读取，如果长度不足，则需要加入等待队列
* init 仿照stdin

###练习二 完成基于文件系统的执行程序机制的实现

> UNIX的硬链接和软链接机制

* 软连接，设置type为SFS_TYPE_SOFT_LINK，可以在indirect中存储文件的对应的连接文件的inode数据块索引，对该inode的操作重定向到指向的inode
* 硬连接，设置type为SFS_TYPE_HARD_LINK，可以在direct[0]中存储硬连接的文件的inode数据块索引，nlinks存储硬连接的数目，索引值的前nlinks项存储硬连接的那些inode的索引，无论存储在direct中，还是indirect指向的索引块中，前nlinks个索引始终为硬连接的文件的索引。当要修改文件时，nlinks个inode索引指向的数据都要修改。同时，建立新的硬连接时，每个inode中的索引项都要更新。