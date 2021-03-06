# 处理机调度

## 处理机调度概念

### CPU资源的时分复用

- 进程切换：CPU资源的当前占用者切换
- 处理机调度
  - 从就绪队列里面挑选下一个占用CPU运行的进程
  - 从多个CPU中挑选一个可以供就绪进程使用的CPU资源
- 调度程序：挑选就绪进程的内核函数
  - 调度程序需要关注两个方面的问题
    1. 调度策略：依据什么原则挑选进程/线程
    2. 调度时机：挑选好的进程/线程，什么时候进行切换
  - 调度时机
    1. 当某一个进程从运行状态切换到等待状态的时候，这个时候CPU会从运行变成空闲的状态，此时内核会运行调度程序
    2. 或者进程被终结了，此时内核会运行调度程序，以上两种情况通常出现在__非抢占式系统__中
    3. 中断请求被响应完成的时候，当前的进程被强占，这种情况通常出现在__抢占式系统__中（例如某一进程的时间片用完，时钟中断使得该进程由运行状态转到就绪状态；或者某一优先级更高的进程从等待状态转到就绪状态，也会抢占正在运行的进程）

## 调度准则

### 调度策略：如何从就绪队列中选择下一个进程执行

### 比较调度算法的准则

- CPU使用率
- 吞吐量：单位时间内完成进程的数量
- 周转时间：进程从初始化到完成的总共用时
- 等待时间：进程在就绪队列中的时间
- 响应时间：从提交请求到返回相应的时间
- 打个比方，当我用水管接水这一类比来说：当我口很渴想喝水的时候，打开水龙头，马上就有水流出来强调的是响应时间很快；当我想要接满一桶水的时候，我不关心开始的时候水来的快不快，我关注的是接满一桶水的时间是否足够的短，这时候我关注的就是水管的吞吐量

#### 调度算法的要求：希望更快的服务，那么如何来定义更快

- 传输文件的时候更高的带宽就是更快，这时候强调调度算法的高吞吐量
- 玩游戏的时候更低的延迟就是更快，这时候强调调度算法的响应时间

##### 处理机调度策略的响应时间目标

- 减少响应时间：尽快将输出的结果反馈给用户
- 减少平均响应时间的波动：在交互系统中，可预测性比高差异低平均更加重要

##### 处理机调度策略的吞吐量目标

- 减少开销（操作系统开销，上下文切换）
- 系统资源的高效利用，尽量让CPU和I/O设备能够持续满状态运行
- 减少等待时间

##### 处理机调度策略的公平性目标

公平性从如下两个角度来定义：

1. 每个进程占用CPU的时间相同
2. 每个进程等待的时间相同

追求公平性会增加进程的平均响应时间

## 调度算法

### 先来先服务算法，FCFS

- 依据进程进入就绪队列的顺序排列
- 优点：简单
- 缺点：
  - 平均等待时间波动比较大（当短进程排在长进程的后边的时候）
  - I/O和CPU资源的利用效率低

### 短进程优先算法

- 选择就绪队列中执行时间最短的进程占用CPU
- 优点：短进程优先算法具有最优的平均周转时间
- 缺点：
  - 产生饥饿现象
  - 需要预知未来

### 高响应比优先算法

- 选择就绪队列中R值最大的进程占用CPU，R = （waiting time / service time）/ service time
- 说明等待的时间越长，进程的优先级越高；进程占用CPU的时间越短，进程的优先级越高，同时考虑了两个因素

### 时间片轮转算法

- 时间片：分配处理机资源的基本时间单元
- 时间片轮转算法的开销：上下文切换
- 时间片太大：算法退化为先来先服务算法
- 时间片太小：大部分的时间都会被消耗在上下文切换的过程中

### 多级反馈队列算法

### 公平共享调度算法

![process scheduling](./pics/process_scheduling.jpg)

## 实时操作系统

- 定义：正确性依赖于时间和功能两方面的操作系统

- 实时操作系统的性能指标：时间约束的即时性需要得到相应的保证，但是速度和平均性能并没有那么重要

  ![real time scheduling](./pics/real_time_scheduling.jpg)

## 多处理机调度

​	![multi processor scheduling](./pics/multi_cpu_scheduling.jpg)