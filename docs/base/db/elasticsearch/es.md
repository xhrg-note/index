## ES(elasticsearch)


#### 单机docker安装ELK


搭建elasticsearch7.12.0

```
#官网网站搜索es，查看tag找到如下命令
1. https://hub.docker.com/ 

#这里的elasticsearch是镜像名称，7.12.0是tag,也是版本
2. docker pull elasticsearch:7.12.0 

#-e是环境变量，-itd，-p是端口映射，--name是指定本地容器名称，最后是镜像id
3. docker run -e ES_JAVA_OPTS="-Xms1024m -Xmx1024m" -e "discovery.type=single-node" -itd -p 9200:9200 -p 9300:9300 --name elasticsearch elasticsearch:7.12.0

4. 访问：http://127.0.0.1:9200/

5. curl -u elastic:changeme localhost:9200
```



搭建kibana7.12.0
```
1. docker pull kibana:7.12.0

2. docker run -d -p 5601:5601 --link elasticsearch -e ELASTICSEARCH_HOSTS=http://elasticsearch:9200 --name kibana kibana:7.12.0

3. 访问：http://127.0.0.1:5601/
```

搭建logstash7.12.0


-e I18N.LOCALE=zh-CN



#### 备忘

> sudo docker pull sebp/elk
> sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk