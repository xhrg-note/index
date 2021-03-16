## 下载

zookeeper下载主页：https://zookeeper.apache.org/releases.html#download 

我下载的是：https://mirrors.ocf.berkeley.edu/apache/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz



## 单机安装

* 下载得到  apache-zookeeper-3.6.2-bin.tar.gz

* 解压进入conf目录后把zoo_sample.cfg复制一份叫做zoo.cfg，然后我们的修改都是基于zoo.cfg，而zoo_sample.cfg相当于一个原始的备份，实际用不到。
* 进入bin目录运行命令启动zookeeper,执行> ./zkServer.sh start  
* 运行客户端连接zookeeper, >   ./zkCli.sh 
* 启动成功后，ls  /  查看数据。ls后跟着斜杠。

