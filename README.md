# usking-tracing-log

#### 介绍
在springboot开发REST接口的项目中，在测试或生产环境，有许多用户访问,日志一块按顺序输出,排查问题很不方便。
此插件用来给给每个Request添加一个trace id,同一个请求的日志trace id相同，这样就方便跟踪问题，能再许多不规则的日志快速找到同一个请求打印的日志。
无代码侵入，加入、卸载方便。按说明配置即可。

#### 软件架构
usking-tracing-log 使用的技术：

1. 使用springboot-starter的原理实现了按条件加载。
2. 利用Filter拦截技术，对请求埋点。

#### 安装教程

1. 引入usking-tracing-log-1.0.1.jar
```xml
<dependency>
    <groupId>top.usking</groupId>
    <artifactId>usking-tracing-log</artifactId>
   <version>1.0.1</version>
</dependency>
```
2. 配置参数
* application.properties 格式配置
```properties
# 启用跟踪日志，默认值false
top.usking.trace.log.trace-log-enabled=true
# 默认跟踪日志启用标志，true-参数没有traceId或header里没有Transaction-ID的情况下打印
top.usking.trace.log.default-trace-log-enabled=true
```

* application.yml 格式配置
```yaml
top:
  usking:
    trace:
    log:
    trace-log-enabled: true
    default-trace-log-enabled: true
```
<font color=#FF0000 >__注意：__ 设置了trace-log-enabled=true不一定能打印出traceId，还需要调用接口的时候在Request里添加traceId参数及值，或在header里添加参数Transaction-ID及值。如果不传参也想打印，需要配置default-trace-log-enabled=true，程序会打印UUID格式的trace id</font>

3. logback-spring.xml 加入traceId参数
具体配置可以参考本工程的logback-spring.xml文件，或直接copy到自己的工程下面即可。
```` xml
  <property name="CONSOLE_LOG_PATTERN"
              value="${CONSOLE_LOG_PATTERN:-%clr(%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd HH:mm:ss.SSS}}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %clr([%X{traceId}]) %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
   
````
4. 如果想把traceId 返回给前端，请使用TraceIdUtil.getTraceId()直接获取。


* log日志如下

```` log
2023-11-22 18:52:06.076  INFO 30228 --- [nio-8080-exec-1] top.sky.test.controller.HelloController  : [00c320a2-5f1f-4e17-9781-74c96cd930d7] Say method starting..........
2023-11-22 18:52:06.076  INFO 30228 --- [nio-8080-exec-1] top.sky.test.controller.HelloController  : [00c320a2-5f1f-4e17-9781-74c96cd930d7] Say to 1234
````

#### 版本说明
* 1.0.0 发布初版
* 1.0.1 添加使用说明文档
