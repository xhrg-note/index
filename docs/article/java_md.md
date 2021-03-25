我们在写中间件的时候，经常会遇到各种传递参数，比如通过启动参数传递数据库链接信息等等，而这些参数对很多人来说，有很大的不确定性。故而做此篇，当我遇到一些好的java项目，都会把其手法写进来。以备学习和使用。

本文会覆盖以下内容，顺序会项目启动时候的可能顺序。

* shell启动脚本
* java启动参数
* maven
* spring(springboot)
* log4j2

## shell启动脚本
总体来说，shell脚本是通过不断组装java启动参数来实现，日志，数据源，端口等信息的改变，而这些改变，实际是和java启动参数。spring都有关的。


shell文件一般都是 startup.sh 和  shutdown.sh 内容一般有以下手法
```
## JAVA_OPTS表示系统上下，一般系统很喜欢用export去定义 JAVA_OPTS等很多参数，并且还会不断追加参数，
export JAVA_OPTS="$JAVA_OPTS -XX:ParallelGCThreads=4“

##  如果有设置SETVER_PORT 环境变量就用。如果没有就使用默认值 8080，并且把 SERVER_PORT 环境变量也设置成 8080 
SERVER_PORT=${SERVER_PORT:=8080}

shell脚本可以把很多参数定义成变量，在后面使用。

传递端口
export JAVA_OPTS="$JAVA_OPTS -Dserver.port=$SERVER_PORT "


传递日志，LOG_DIR和SERVICE_NAME都是前面定义的变量，-Dlogging.file会直接传递给springboot
-Dlogging.file=$LOG_DIR/$SERVICE_NAME.log
```

## java启动参数
java启动的时候  java -Dname=value，这个时候，会把name=value这个java系统(注意这里并不是你配置的环境变量)变量放入系统上下文，在进程中是可以拿到的。

* -Dserver.port=$SERVER_PORT ，传入server.port=8080到上下文
* -Dspring.datasource.url=$DS_URL 传入spring.datasource.url=xxx到上下文

```
一般取环境变量有2种System.getProperties()(一般成为java环境变量);和System.getenv()(一般成为操作系统环境变量);
配置的环境变量一般是取getenv，而getProperties取的是一些系统参数，
这里需要注意的是jvm的-Dkey=value这个值是从getProperties中拿，因此，一般先拿getProperties后拿getenv

spring的类PropertyPlaceholderConfigurer中如下也能看出先后顺序

protected String resolveSystemProperty(String key) {
		try {
			String value = System.getProperty(key);
			if (value == null && this.searchSystemEnvironment) {
				value = System.getenv(key);
			}
			return value;
		}
```

## maven
* dependencyManagement ，pom文件配置了这个在内部定义很多dependencies，这样子项目引用就不需要写版本了。
* build/pluginManagement，build下的/pluginManagement也是同样作用。子项目引用就不需要写版本了。
## spring(springboot)

我们知道springboot有一个配置文件是application.yml或者properties，如果里面有${xxxx}这样的内容，其中这个就可以拿到java启动参数的上下文变量。

以下是springboot常用的配置文件信息：


--

#### org.springframework.context.annotation.Profile

这个注解比较有意思，先说这个注解的使用
```
//一般放在类上，这样，在spring的application中有这样一个字段，spring.profiles.active=xxx-dev,uat,xxx,aaa
//如果这里包含了xxx-dev，则这个类的配置是启动的，否则不启动。
@Configuration
@Profile("xxx-dev")
public class WebContextConfiguration
```

## log4j2