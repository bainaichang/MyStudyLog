# 进程

父进程 -> 调用fork() -> 恢复执行

fork() -> exec() 将可执行文件载入地址空间

task_struct 用于描述进程

tasklist 是一个双向循环链表

2.6内核前 每个进程的task_struct 存放在内核栈的尾端

```c
struct thread_info {
    struct task_struct *task;
    ...
}
```

通过 current_thread_info()->task 拿到当前 task_struct

## 进程的状态

```
TASK_RUUING 运行
TASK_INTERRUPTIBLE 可中断的睡眠
TASK_UNINTERRUPTIBLE 不可中断的睡眠
__TASK_TRACED 被其他进程跟踪的进程
__TASK_STOPPED 停止
```

## 进程树

每个task_struct都有一个指向父进程的指针

```c
struct task_struct *my_parent = current -> parent;
```

而子进程是一个链表

## Unix的进程创建

首先fork(),然后exec()

## 写时拷贝

Linux中的fork()只有在需要写入时才进行新的拷贝,否则共用一个拷贝

## fork()

Linux通过clone()实现fork(),然后通过给clone()传递参数表示父子进程之间共享的资源

fork(),vfork(),__clone() 都要传递不同的参数以调用chone(), clone()调用do_fork().do_fork()调用copy_process()

总结一下: fork()->clone()->do_fork()->copy_process()

## do_fork()

1.   调用dup_task_struct()为新的进程创建一个内核栈,thread_info结构和task_struct,这些值与当前进程的值相同
2.   检查新进程,以确保用户的资源没有超出范围
3.   开始进行赋值
4.   将新子进程的状态设为TASK_UNINTERRUPTIBLE 不可被打断的睡眠 以确保它不会被运行
5.   copy_process()调用copy_flags()以更新task_struct的flags成员.表明进程是否有root权限的PF_SUPERPRIV被清0.表明进程还没调用exec()的PF_FORKNOEXEC标志被设置
6.   调用alloc_pid()分配一个PID
7.   根据传递给clone()的参数,copy_process()开始拷贝或共享资源
8.   copy_process()做扫尾工作然后返回一个子进程的指针

如果copy_process()成功执行,do_fork()把新进程唤醒并投入运行,一般子进程先执行,马上调用exec()

## vfork()

一样