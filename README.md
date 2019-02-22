# log

## springboot日志配置

> demo参见[industry-map](../industry-map) 

+ 在`resources`目录下添加`logback-spring.xml`

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <configuration>
  
      <include resource="org/springframework/boot/logging/logback/defaults.xml" />
      <include resource="org/springframework/boot/logging/logback/console-appender.xml" />
  
      <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
          <File>${LOG_PATH}${LOG_FILE}</File>
          <encoder>
              <pattern>%date [%level] [%thread] %logger{60} [%file : %line] %msg%n</pattern>
          </encoder>
          <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
              <!-- 添加.gz 历史日志会启用压缩 大大缩小日志文件所占空间 -->
              <fileNamePattern>${LOG_PATH}daily/${LOG_FILE}.%d{yyyy-MM-dd}.gz</fileNamePattern>
              <maxHistory>30</maxHistory><!--  保留30天日志 -->
          </rollingPolicy>
      </appender>
  
      <root level="INFO">
          <appender-ref ref="CONSOLE"/>
          <appender-ref ref="FILE"/>
      </root>
  </configuration>
  ```

  这个文件可以原封不动使用

+ 在`application.properties`中配置

  ```properties
  # 日志
  logging.level.root=info
  logging.level.com.rjs.flowable=debug
  logging.path=logs/
  logging.file=reception.log
  ```

  这里的配置是给上面的xml文件使用的

+ 使用

  在需要使用的类中先获取logger对象，然后打印

  ```java
  class A{
      private final Logger logger = LoggerFactory.getLogger(A.class);
      public void test(){
          logger.error("msg",e);
      }
  }
  ```


## 知识点

### log.error(e)与log.error(e,e)区别

> 参见[log.error(msg)和log.error（msg,e）的显示区别](https://www.cnblogs.com/toSeeMyDream/p/7685835.html) 

前者只打印异常信息，后者同时打印堆栈信息

但是一般不这么用，一般第一个参数打印自定义字符串，e放在第二个参数