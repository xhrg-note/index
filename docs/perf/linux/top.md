## top


运行top出现如下信息，其中行号是我加的，为了说明方便：
```
1. top - 14:01:10 up 67 days, 20 min,  2 users,  load average: 2.70, 2.34, 2.29
2. Tasks: 113 total,   2 running,  72 sleeping,   0 stopped,   0 zombie
3. %Cpu(s):  1.3 us,  2.3 sy,  0.0 ni, 96.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
4. KiB Mem :  1877192 total,   230756 free,   780924 used,   865512 buff/cache
5. KiB Swap:        0 total,        0 free,        0 used.   919488 avail Mem 

6. PID  USER  PR  NI VIRT     RES    SHR   S  %CPU  %MEM  TIME+    COMMAND
7. 8277 root  20  0  1009452  60616  14880 S  1.3   3.2   99:45.53 YDService
8. 8287 root  20  0  653644   15132  12420 S  0.7   0.8   18:52.01 YDEdr
```

* 解释说明

```
1. 系统信息
  * 14_01_10 当前时间, 
  * up 67 days 系统运行了67天, 
  * 20 min 待定，
  * 2 users 当前登录用户数, 
  * load average: 2.70, 2.34, 2.29 系统负载百分比，即任务队列的平均长度。三个数值分别为 1分钟、5分钟、15分钟前到现在的平均值。
2. 任务信息
  * total 进程总数，
  * running 正在运行的进程数，
  * sleeping 睡眠的进程数，
  * stopped 停止的进程数，
  * zombie 僵尸进程数
3. CPU相关百分比
  * 1.3 us 用户空间占用CPU百分比
  * 2.3 sy 内核空间占用CPU百分比
  * 0.0 ni 用户进程空间内改变过优先级的进程占用CPU百分比
  * 96.3 id 空闲CPU百分比
  * 0.0 wa 等待输入输出的CPU时间百分比
  * 0.0 hi：硬件CPU中断占用百分比
  * 0.0 si：软中断占用百分比
  * 0.0 st：虚拟机占用百分比
4. KiB Mem, 物理内存,单位kb
  * 1877192 total 物理内存总量
  * 230756 free 空闲内存总量
  * 780924 used 使用的物理内存总量
  * 865512 buff/cache 用作内核缓冲和缓存的内存量
5. 进程信息
  * PID 进程ID
  * USER 运行该命令的用户
  * PR 优先级
  * NI nice值，越大表示优先级越低，负数表示优先级高
  * VIRT 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
  * RES 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA 
  * SHR 共享内存大小，单位kb 
  * S 进程状态(D=不可中断的睡眠状态,R=运行,S=睡眠,T=跟踪/停止,Z=僵尸进程)
  * %CPU 上次更新到现在的CPU时间占用百分比
  * %MEM 进程使用的物理内存百分比
  * TIME+ 进程使用的CPU时间总计，单位1/100秒
  * COMMAND 命令名/命令行
```

## top更多使用

* top后按下数字1，就能看到每个cpu了，也可以知道是几核cpu
* top -c 显示命令的完整路径
* top -c -p pid 只显示某一个进程信息


## top交互
* z 显示颜色
* e 切换下面的单位，
* E 切换上面的单位 

