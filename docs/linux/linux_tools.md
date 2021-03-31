## 系统信息

```
cat /proc/meminfo  #查看内存信息
cat /proc/version  #查看linux系统版本
cat /proc/cpuinfo  #查看linux的cpu信息
```

## 内存

```
top 然后看%MEM
free -h #查看内存
cat /proc/meminfo #查看内存信息
```

## 文件磁盘

```
df -Th #查看文件目录大小
df -h
du
```

## 网络

```
cat /proc/pid/limits #查看pid这个进程的limit限制
```

## netstat
```
# 根据进程id查看进程占用端口:
netstat -nap | grep 1095


# 根据端口查看对应进程
netstat -tunlp | grep 8080
```

## curl

* 注意curl -v 的确可以显示详情，但是有一些很少的情况下，他会把显示的内容和http的返回数据混合。

简单例子  curl -v http://localhost:80/hello

curl常用参数

* -v 显示通信过程
* curl -X POST --data "data=xxx" example.com/form.cgi
* curl -X POST--data-urlencode "date=April 1" example.com/form.cgi

curl问题记录

```
xxxx@xxxx:/xxxx/xxxx# curl -v http://localhost:80/hello
*   Trying ::1...
* connect to ::1 port 80 failed: Connection refused
*   Trying 127.0.0.1...
* Connected to localhost (127.0.0.1) port 80 (#0)
> GET /hello HTTP/1.1
> Host: localhost
> User-Agent: curl/7.47.0
> Accept: */*
< HTTP/1.1 200 OK
< Server: xxxx
< Date: Sun, 27 Sep 2020 08:01:44 GMT
< Content-Type: text/plain; charset=utf-8
< Content-Length: 70
< 
* Connection #0 to host localhost left intact
{"message":"Hello , I am service"}
//如上，根据HTTP/1.1 200 OK和message知道返回是正确的，
//但是port 80 failed: Connection refused是为何
```


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


## tar

```
压缩
tar -zcvf test.tar.gz nohup.no 

解压
tar -zxvf test.tar.gz
```

##end