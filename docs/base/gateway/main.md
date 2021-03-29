
## 网关


一般情况下，一个公司的域名是这样的 www.xxx.com ,公司内部的项目非常多，可能有N个部门，其中每个部门又要很多项目，每个项目又有很多http接口。那么这个时候，对外的接口可能是这样的

* www.xxx.com/order-service/queryOrder
* www.xxx.com/person-service/login
* www.xxx.com/item-setvice/item

等等，如果要增加新的接口，就需要运维去配置，可是管理一个域名的运维要经常改nginx风险很大，而且效率非常低下。这个时候，如果有一个网关可以帮助业务自行配置接口就非常好增加c端的接口。



## 网关流量多重模式

* nginx -> gateway -> http-service。如果用这种模式，一般是http请求到nginx，nginx到网关, 网关到后台http服务，然后把熔断限流等公共需求放在gateway中，这种模式有一个好处是可以把请求透传到后台。缺点是，对于业务来说，收益不是很高，因为从nginx->http-service之间，有没有gateway对业务来说并没有太大好处。

* nginx -> gateway -> rpc-service。 如果用这种模式，也是可以把熔断限流等公共服务放在gateway，这种模式有一个好处是业务使用的时候，没有http层，他们会觉得节约了一个节点，省掉了服务相关的代码，部署等。但是有一个缺点是，省去了http层的控制。

* nginx[openresty] -> http-service【我个人推崇】。 这种模式是把nginx换位基于openresty的网关，网关该有的功能也有，最大的缺点是openresty的扩展不好做，而且公司内部人员流动后面不好维护。

* bff。


## 市面上的网关
* [manba-golang](https://github.com/fagongzi/manba)
* [kong-openresty](https://github.com/Kong/kong)
* [soul-java](https://github.com/dromara/soul)
* [apisix-openresty](https://github.com/apache/apisix)