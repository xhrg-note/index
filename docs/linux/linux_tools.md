# 常用命令

#### 根据 进程 id查看进程占用端口:

> netstat -nap | grep 1095

#### 根据 端 口查看对应进程

> netstat -tunlp | grep 8080





## wrk

* wrk -d10m -t128 -c3000  -H"Host: www.domain.com" http://127.0.0.1:8000/query
