

## rocketmq安装

## 下载 

http://mirrors.tuna.tsinghua.edu.cn/apache/rocketmq/4.4.0/rocketmq-all-4.4.0-bin-release.zip


## 启动部署台

* 先启动nameserver

```
>>bin/nameserver
```

* 启动broker
```
>>bin/mqbroker -n localhost:9876 
>>nohup sh mqbroker -n 127.0.0.1:9876 autoCreateTopicEnable=true > broker_log.log 2>&1 &
```

## 编写简单java代码

* maven


```
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.4.0</version>
</dependency>
```



```java
public class MqConsumerClient {
    public static void main(String[] args) throws  Exception {
        DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("ConsumerGroup");
        consumer.setNamesrvAddr("127.0.0.1:9876");
        consumer.subscribe("topicName","*");
        consumer.registerMessageListener(new MessageListenerConcurrently(){
            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> list, ConsumeConcurrentlyContext consumeConcurrentlyContext) {
                System.out.println(list);
                System.out.println(consumeConcurrentlyContext);
                return null;
            }
        });
        consumer.start();
        System.out.println("消费者启动");
    }
}
```



```
public class App {
	public static void main( String[] args ) throws 	Exception {
    	 DefaultMQProducer producer = new DefaultMQProducer("ConsumerGroup");
         producer.setNamesrvAddr("127.0.0.1:9876");
         producer.start();
         producer.setSendMsgTimeout(30000);
         for (int i = 0; i < 50000000; i++) {
             Message msg = new Message("topicName" ,("Hello_RocketMQ " + i).getBytes("UTF-8"));
             SendResult sendResult = producer.send(msg);
             System.out.printf("%s%n", sendResult);
             Thread.sleep(3000);
         }
         System.out.println("生产者发送了");
         producer.shutdown();
    }
}
```

