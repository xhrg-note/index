*本文会不断更新,包括增加一些好的中间件,包括对分类更合理的划分。*

# 基础架构作用

基础架构有3个问题比较核心的问题：：：

* 最终用户。用户需要的是可扩展(功能升级),稳定性(可用,稳定),漂亮易用。
* 开发人员。
  1. 简单上手。更简单的使用各种技术,而无需投入太多学习成本。
  2. 项目稳定,高可用,可扩展,可观测。包括微服务,分布式,监控告警等等,
  3. 深入关注业务功能,业务发展,产品功能,个人绩效。
  4. 员工还关注技术分享和技术沉淀,所以内部分享也很重要,包括技术入门,业务痛点,解决问题,架构设计等等。
  5. 人员成本,开发效率,工作效率。
* 机器成本,服务器资源,第三方软件,还有资源的使用效率等。

本文概括基础架构的所有组件,以此扩展,深挖,希望读者评论本文。可以有分类的建议,中间件的推荐,框架的推荐等。更注重实战,但是好的实战离不开对原理的了解。

[基础软件技术考虑点]

# 技术门户(技术入口)

门户一般用于全公司统一入口,第一就是统一子系统UI,统一登陆,权限认证。子系统也必须有自己多高内聚,低耦合。内聚表现在用户可以一眼看到所有多产品,使用简单,无需学习切换浏览器等,低耦合表现在,子系统可以独立部署,独立接管权限功能等。

门户的分类可以参考阿里云,腾讯云等。但是比他们更多,因为设计到公司内部流程系统,工具到。一个可能到分类如下(一定要注意,产品化公司等任何项目产品,而组合一定是可以随意配置的,比如说把配置系统,可以放到任何栏目,或者模板都是可以的)：
* 工作流：审批,请假,资源申请,任务管理(张三可以给李四创建任务)
* 中间件：消息队列,调度,
* 基础设施：日志,告警,监控,配置,
* 运维管理：应用管理,数据库,redis,
* 测试管理：功能测试,压力测试,

# 基础中间件

* 应用信息。包括应用的名称,owner,部门,sdk版本等元信息,需要管理。不然后期推广新框架,做公司治理都会麻烦。
* [消息队列](https://www.jianshu.com/p/9724dc52c44a)。[rocketmq](https://www.jianshu.com/p/aedc88f1d2b8),activemq(新一代叫activemq-artemis),kafka,apache-pulsar,rabbitmq,nats,nsq,[qmq](https://github.com/qunarcorp/qmq),zeromq,Spring Cloud Stream 
* 配置中心。[ctripcorp.apollo](https://www.jianshu.com/p/b39e17a2a77f),[nacos](https://nacos.io),kms(秘钥管理),[brcc](https://github.com/baidu/brcc),disconf,Spring Cloud Config
* [调度平台](https://www.jianshu.com/p/50dd27a87d2b)。[powerjob](https://github.com/KFCFans/PowerJob), xxl-job,elastic-job,vip-saturn
* 数据订阅。alibaba-canal,linkedin-databus, debezium + kafka-connect
* [网关](https://www.jianshu.com/p/c1c660d7353e)。spring-gateway,zuul,kong,apisix,manba(golang),悟空(golang)
* 分布式事务。seata,消息事务,
* [服务框架](https://www.jianshu.com/p/8eb69c6d3c9e)。[dubbo](https://www.jianshu.com/p/76264499c116),grpc,sofa-rpc, octo-rpc(系列), [sentinel](https://sentinelguard.io/zh-cn/docs/quick-start.html)(2020版支持golang), spring-cloud全家桶, Spring Cloud Alibaba, Apache ServiceComb, SOFAStack, Helidon, Quarkus, Lagom, Micronaut, go-micro, 腾讯Tars,Apache BRPC,Motan,服务治理[流控[sentinel,hystrix,Hystrix,Sentinel,Resilience4j]
* 注册中心。zookeeper,etcd,eureka,consul,redis
* 服务网格。sofa-mosn,[微软-OSM](https://github.com/openservicemesh/osm)

# 通用业务
* 【用户中心】SSO登录。
* 【开放平台】外部调用内部的开放平台,内部调用外部的outing项目。
* 【公共服务】动态日志。消息推送平台：短信,邮件,微信,钉钉,飞书。

# 基础设施
* 【监控】Logging。elk,loki,https://github.com/grafana/loki
* 【监控】Tracing。pinpoint,skywalking,eagleeyes,appdynamic,oneapm,pinpoint,dianping-cat,[didi-nightingale](https://n9e.didiyun.com),zipkin,jaeger
* 【监控】Metrics。时序相关的,[prometheus](https://www.jianshu.com/p/fdbb17278da4) + Grafana,datadog,Open-Falcon
* 【监控】其他。zabbix

# 数据中间件
* 分布式缓存。[redis](https://www.jianshu.com/p/b6f6c100da54)[cluster,sentinel,codis],hazelcast
* 分布式数据库。[tidb](https://pingcap.com/)
* 数据库中间件(框架)。shardingsphere,dble,[kingshard(go)](https://github.com/flike/kingshard/blob/master/README_ZH.md)
* 分布式文件系统。fastdfs,seaweedfs,bookkeeper
* 数据库。mysql, 注意, 很多业务开发的同学,基本上都在和mysql打交道,所以mysql相关的技术是非常重要的。

# 数据中台
* BI系统。Davinci
* 调度系统。hera,DolphinScheduler
* 数据存储。hadoop
* 实时计算。clickhouse(单表)
* 离线计算。
* 数据采集和数据传输。[datax](https://github.com/alibaba/DataX) + [datax-web](https://github.com/WeiYe-Jing/datax-web)

# 运维管理
* 发布平台|tars|jenkins|KubeOperator|Rundeck|opendevops|walle-web.io|Travis
* [rancher](https://rancher.com/docs/rancher/v1.6/zh/)
* [宜信-kplcloud](https://github.com/kplcloud/kplcloud)
* k8s
* 18款工具,https://mp.weixin.qq.com/s/CAroslMhKt21y6_XYYXqQg
* 服务申请平台。服务器,mysql,zk等资源申请平台。

# 开发效率
* 代码生成器。[code-gen](https://gitee.com/durcframework/code-gen)
* 基础框架,工具类
* 业务开发流程化标准平台。需求->创建项目->资源申请->上线->监控->下线
* 企业开发平台(包括用户,权限,代码生成,服务治理,各种开发用到的工具继承进去)

# 框架组件

对于业务开发的同学,不可避免的用一些性能不高的类,比如说喜欢用StringBuffer,而非StringBuilder,所以基础框架需要不断深入研究底层数据结构,让业务开发使用公司公共组件类,而非jdk或者开源组件。没必要花时间去研究下所有的类的数据结构高效性。由基础框架统一规划,文档输出。

* 常用类数据结构的封装。对于业务开发的同学,有时候不可避免用一些性能不好
* 短信|邮件|钉钉等第三方调用。
* 分布式锁。
*  guava->本地缓存,本地限流
*  apache-common -> 对象池
*  [jushata](https://github.com/didi/JuShaTa)

# 测试相关
* 自动化测试。robotframework
* 压力测试平台。apache-jmeter,https://github.com/smallnest/go-web-framework-benchmark,wrk
* 破坏测试平台。ali-chaosblade

# 大数据
* https://gitee.com/jman325_admin/pagenow_open

# 工具中心
* arthas
* jdk中bin目录 
* [linux性能优化](https://www.jianshu.com/p/de15f520d030)
* [java性能优化](https://www.jianshu.com/p/5f57d4190c2f),包括gc算法,gc收集器,内存,等等所有java相关。

# 统一规范
* [日志,分之,编码,等的规范](https://www.jianshu.com/p/168c10fd7747)
* 编码规范。
* 项目结构规范。
* 日志规范
* 接口文档。swagger[[knife4j](https://gitee.com/xiaoym/knife4j)] 

# 统一UI
一个好的中间件产品,怎么能少不了一套好的ui呢。
* layUI
* icework
* ant.design