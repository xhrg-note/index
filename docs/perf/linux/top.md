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


1. 系统信息
  * 14:01:10 当前命令的时间, 
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
  * 0.3% us 用户空间占用CPU百分比
  * 1.0% sy 内核空间占用CPU百分比
  * 0.0% ni 用户进程空间内改变过优先级的进程占用CPU百分比
  * 98.7% id 空闲CPU百分比
  * 0.0% wa 等待输入输出的CPU时间百分比
  * 0.0%hi：硬件CPU中断占用百分比
  * 0.0%si：软中断占用百分比
  * 0.0%st：虚拟机占用百分比
```


