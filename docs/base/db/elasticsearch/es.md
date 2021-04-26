## ES(elasticsearch)




#### 单机docker安装es

只为了简单

```
#官网网站搜索es，查看tag找到如下命令
1. https://hub.docker.com/ 

#这里的elasticsearch是镜像名称，7.12.0是tag,也是版本
2. docker pull elasticsearch:7.12.0 

#-e是环境变量，-itd，-p是端口映射，--name是指定本地容器名称，最后是镜像id
3. docker run -e ES_JAVA_OPTS="-Xms1024m -Xmx1024m" -e "discovery.type=single-node" -itd -p 9200:9200 -p 9300:9300 --name my_elasticsearch_7.12.0 elasticsearch:7.12.0

4. 访问：http://127.0.0.1:9200/
```


```
docker pull kibana:7.12.0


# 这个ip地址，需要用es的ip
docker run -d --name kibana_7.12.0 -p 5601:5601 --link my_elasticsearch_7.12.0 -e ELASTICSEARCH_HOSTS=http://172.17.0.3:9200 kibana:7.12.0
```