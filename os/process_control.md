# 进程控制

## 进程切换（上下文切换）

- 暂停当前运行的进程，从运行状态变成其他状态
- 调度另一个进程从就绪状态变成运行状态

![进程切换](./pics/content_switch.jpg)

### 进程切换的要求

- 切换前，保存上下文
- 切换后，恢复上下文
- 快速切换

### 进程生命周期的信息

- 寄存器（PC，SP）
- CPU状态
- 内存地址空间

### 进程控制块PCB：内核的进程状态记录

- 内核为每个进程维护了对应的PCB
- 内核把状态相同的PCB放到同一个队列里面

![进程控制块中所包含的信息](./pics/process_struct.jpg)

## 进程创建

- Windows的系统调用：CreateProcess(filename)
- Unix的系统调用：fork/exec
  - fork()把一个进程复制成两个进程：parent(old PID), child(new PID)
    - fork()复制父进程的所有内存和变量
    - fork()复制父进程的所有CPU寄存器（有一个寄存器例外）
    - 子进程的fork()返回0，父进程返回进程标识符
    - fork()的返回值可以方便后续使用
  - exec()把一个新的程序加载到进程当中，重写当前进程



## 进程加载

## 进程等待和退出

### 进程等待

- wait()系统调用用于父进程等待子进程
  - 子进程结束的时候通过exit()向父进程返回一个值
  - 父进程通过wait()接受并处理返回的值
- wait()系统调用的功能
  - 有子进程活动的时候，父进程进入等待的状态，等待子进程返回的结果。当子进程执行exit()的时候，唤醒父进程，将exit()的值作为wait()的返回值
  - 有僵尸子进程等待的时候，wait()立即返回其中的一个值
  - 无子进程存活的时候，wait()立即返回一个值

### 进程退出

- 进程结束，执行调用exit()，完成进程的资源回收
- exit()系统调用的功能
  - 将调用参数作为进程的“结果”
  - 关闭所有打开的文件
  - 释放内存
  - 释放大部分进程相关的内核数据结构
  - 检查父进程是否存活着
    - 如果存活着，则保留exit()的返回值，直到父进程需要用的时候，进入僵尸(zombie/defunct)状态
    - 如果没有，释放所有的数据结构，进程结束
  - 清理所有等待的僵尸进程

## 其他进程控制的系统调用

- 优先级控制：nice()
- 进程调试支持：允许一个进程控制另一个进程的执行，如设置断点和查看寄存器信息
- 定时：sleep()可以让进程在定时器的等待队列中等待指令

## 总览

![进程控制与进程状态的关系](./pics/process_ctrl.jpg)