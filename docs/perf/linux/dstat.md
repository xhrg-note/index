## dstat

很强大的一个工具，dstat可以取代的工具有
* vmstat
* iostat
* netstat
* ifstat
----

用法介绍


## 直接输入dstat

```
--total-cpu-usage-- -dsk/total- -net/total- ---paging-- ---system--
usr sys idl wai stl| read  writ| recv  send|  in   out | int   csw 
  0   0  99   0   0|4115B   34k|   0     0 |   0     0 | 262   775
```

1. --total-cpu-usage---- CPU使用率
* usr：用户空间的程序所占百分比；
* sys：系统空间程序所占百分比；
* idel：空闲百分比；
* wai：等待磁盘I/O所消耗的百分比；
* hiq：硬中断次数；
* siq：软中断次数；
2. -dsk/total-磁盘统计
* read：读总数
* writ：写总数
3. -net/total- 网络统计
* recv：网络收包总数
* send：网络发包总数
4. ---paging-- 内存分页统计
* in： pagein（换入）
* out：page out（换出）
5. --system--系统信息
* int：中断次数
* csw：上下文切换

