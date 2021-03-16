# 常用工具

#### 根据 进程 id查看进程占用端口:

> netstat -nap | grep 1095

#### 根据 端 口查看对应进程

> netstat -tunlp | grep 8080




## wrk

安装例子

```
git clone
```

常用例子

```
wrk -t4 -c1000 -d30s --latency www.baidu.com

wrk -d10m -t64 -c2000  -H"Host: www.domain.com" http://127.0.0.1:8000/demo/query
//注意这里的Host: www.domain.com中间有个空格
```

参数说明

```
-t --threads   开启的线程数，用于控制并发请求速度。最大值一般是cpu总核心数的2-4倍
-c --conections 保持的连接数（会话数）
-d --duration 压测持续时间(s)
-s --script 加载lua脚本（post请求写一些参数到脚本里）
-H --header 在请求头部添加一些参数
--latency 压测报告输出请求回包花费时间分布
--timeout 请求的最大超时时间(s),这个很有用
```


## ab


```
基本用法：：：
ab -n 100 -c 10 http://test.com/
其中－n表示请求数，－c表示并发数

用例：：：
ab －n 100 －C key＝value http://test.com/  # -C表示Cookie
ab -n 100 -H “Cookie: Key1=Value1; Key2=Value2” http://test.com/ # -H 表示header

参数：：：：
-n:总共的请求执行数，缺省是1；
-c:并发数，缺省是1；
-t:测试所进行的总时间，秒为单位，缺省50000s
-p:POST时的数据文件
-w:以HTML表的格式输出结果 

```