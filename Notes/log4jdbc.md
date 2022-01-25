# 问题

在进行开发时，偶尔会想知道自己的查询结果是什么，参数有没有传错，
对于原生语句，下面一个简单的getOne查询，控制台的输出是这样的
这样子对我来说，当我想知道id究竟传进入什么值，获得得到的结果集是怎么样的，就显得比较难受

```sql
select user0_.id as id1_1_0_, user0_.age as age2_1_0_, user0_.dept_id as dept_id4_1_0_, user0_.user_name as user_nam3_1_0_, dept1_.id as id1_0_1_, dept1_.dept_name as dept_nam2_0_1_ from sys_user user0_ left outer join sys_dept dept1_ on user0_.dept_id=dept1_.id where user0_.id=?

```

# 引入框架log4jdbc

为了解决上面问题，我引入了log4jdbc框架

## pom文件导入



```xml
<!--监控sql日志-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>org.bgee.log4jdbc-log4j2</groupId>
    <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
    <version>1.16</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>${slf4j.version}</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>${logback.version}</version>
</dependency>

```

## 修改数据源的配置

```yaml
spring:
  datasource:
      # 这里在jdbc与mysql之间加入log4jdbc，注意冒号
      url: jdbc:log4jdbc:mysql://localhost:3306/jpa?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
      username: root
      password: 123456
      # 驱动需要进行修改
      driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy

## 原配置
#spring:
#    datasource:
#        url: jdbc:mysql://localhost:3306/jpa?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
#        username: root
#        password: 123456
#        driver-class-name: com.mysql.cj.jdbc.Driver

.........

logging:
  level:
    org.springframework: WARN
    org.spring.springboot.dao: debug
    root: info
  com.*.*.mapper: debug
  # log4jdbc
  jdbc.connection: FATAL
  jdbc.resultset: error
  jdbc.audit: FATAL
  jdbc.sqlonly: debug
  jdbc.sqltiming: DEBUG
  jdbc.resultsettable:
```

## 进行必要的属性配置

==在resources目录下，新建log4jdbc.log4j2.properties属性文件，添加以下配置==

```properties
# 这里采用slf4j日志
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
# 将自动设置驱动关闭
log4jdbc.auto.load.popular.drivers=false
# 设置mysql驱动
log4jdbc.drivers=com.mysql.cj.jdbc.Driver
```

## 我的logback-spring.xml文件

### 详细版

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 日志级别从低到高分为TRACE < DEBUG < INFO < WARN < ERROR < FATAL，如果设置为WARN，则低于WARN的信息都不会输出 -->
<!-- scan:当此属性设置为true时，配置文档如果发生改变，将会被重新加载，默认值为true -->
<!-- scanPeriod:设置监测配置文档是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。 当scan为true时，此属性生效。默认的时间间隔为1分钟。 -->
<!-- debug:当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。 -->
<configuration scan="true" scanPeriod="60 seconds" debug="false">
    <contextName>jpa-select-jpa</contextName>

    <!-- <springProperty scope="context" name="logPath" source="logging.path"
        defaultValue="logs"/> -->
    <!-- name的值是变量的名称，value的值时变量定义的值。通过定义的值会被插入到logger上下文中。定义后，可以使“${}”来使用变量。 -->
    <property name="log.path" value="logs/"/>

    <!--0. 日志格式和颜色渲染 -->
    <!-- 彩色日志依赖的渲染类 -->
    <conversionRule conversionWord="clr"
                    converterClass="org.springframework.boot.logging.logback.ColorConverter"/>
    <conversionRule conversionWord="wex"
                    converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter"/>
    <conversionRule conversionWord="wEx"
                    converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter"/>
    <!-- 彩色日志格式 -->
    <property name="CONSOLE_LOG_PATTERN"
              value="${CONSOLE_LOG_PATTERN:-%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>

    <!--1. 输出到控制台 -->
    <appender name="CONSOLE"
              class="ch.qos.logback.core.ConsoleAppender">
        <!-- 此日志appender是为开发使用，只配置最底级别，控制台输出的日志级别是大于或等于此级别的日志信息 -->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>debug</level>
        </filter>
        <encoder>
            <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
            <!-- 设置字符集 -->
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!--2. 输出到文档 -->
    <!-- 2.1 level为 DEBUG 日志，时间滚动输出 -->
    <appender name="DEBUG_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_debug.log</file>
        <!--日志文档输出格式 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 日志归档 -->
            <fileNamePattern>${log.path}/web-debug-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数 -->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录debug级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>debug</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.2 level为 INFO 日志，时间滚动输出 -->
    <appender name="INFO_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_info.log</file>
        <!--日志文档输出格式 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 每天日志归档路径以及格式 -->
            <fileNamePattern>${log.path}/web-info-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数 -->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录info级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>info</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.3 level为 WARN 日志，时间滚动输出 -->
    <appender name="WARN_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_warn.log</file>
        <!--日志文档输出格式 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 此处设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.path}/web-warn-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数 -->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录warn级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>warn</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.4 level为 ERROR 日志，时间滚动输出 -->
    <appender name="ERROR_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_error.log</file>
        <!--日志文档输出格式 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 此处设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.path}/web-error-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数 -->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录ERROR级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!--监控sql日志输出 -->
    <!--使用绑定参数替换为绑定数据来记录执行的sql-->
    <logger name="jdbc.sqlonly" level="OFF" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>
    <!--记录执行一条SQL所花费的时间-->
    <logger name="jdbc.sqltiming" level="INFO" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>
    <!--  如不想看到表格数据，将INFO改为OFF -->
    <logger name="jdbc.resultsettable" level="OFF" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>
    <!--与jdbc.audit以及ResultsSets相同-->
    <logger name="jdbc.resultset" level="OFF" additivity="false">
        <appender-ref ref="CONSOLE"></appender-ref>
    </logger>
    <!--记录除ResultSets之外的所有jdbc调用-->
    <logger name="jdbc.audit" level="OFF" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>
    <!--记录打开和关闭连接事件-->
    <logger name="jdbc.connection" level="OFF" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>

    <!-- <logger>用来设置某一个包或者具体的某一个类的日志打印级别、 以及指定<appender>。<logger>仅有一个name属性，
        一个可选的level和一个可选的addtivity属性。 name:用来指定受此logger约束的某一个包或者具体的某一个类。 level:用来设置打印级别，大小写无关：TRACE,
        DEBUG, INFO, WARN, ERROR, ALL 和 OFF， 还有一个特俗值INHERITED或者同义词NULL，代表强制执行上级的级别。
        如果未设置此属性，那么当前logger将会继承上级的级别。 addtivity:是否向上级logger传递打印信息。默认是true。 <logger
        name="org.springframework.web" level="info"/> <logger name="org.springframework.scheduling.annotation.ScheduledAnnotationBeanPostProcessor"
        level="INFO"/> -->

    <!-- 使用mybatis的时候，sql语句是debug下才会打印，而这里我们只配置了info，所以想要查看sql语句的话，有以下两种操作：
        第一种把<root level="info">改成<root level="DEBUG">这样就会打印sql，不过这样日志那边会出现很多其他消息
        第二种就是单独给dao下目录配置debug模式，代码如下，这样配置sql语句会打印，其他还是正常info级别： 【logging.level.org.mybatis=debug
        logging.level.dao=debug】 -->

    <!-- root节点是必选节点，用来指定最基础的日志输出级别，只有一个level属性 level:用来设置打印级别，大小写无关：TRACE,
        DEBUG, INFO, WARN, ERROR, ALL 和 OFF， 不能设置为INHERITED或者同义词NULL。默认是DEBUG 可以包含零个或多个元素，标识这个appender将会添加到这个logger。 -->

    <!-- 4. 最终的策略 -->
    <!-- 4.1 开发环境:打印控制台 -->
    <springProfile name="dev">
        <root level="info">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>


    <!-- 4.2 生产环境:输出到文档 -->
    <springProfile name="pro">
        <root level="info">
            <appender-ref ref="CONSOLE"/>
            <appender-ref ref="DEBUG_FILE"/>
            <appender-ref ref="INFO_FILE"/>
            <appender-ref ref="WARN_FILE"/>
            <appender-ref ref="ERROR_FILE"/>
        </root>
    </springProfile>

</configuration>
```

### 简化版

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="30 seconds" debug="false">
    <!--项目名-->
    <contextName>plojectName</contextName>
    <property name="LOG_HOME" value="logs/"/>
    <property name="log.charset" value="utf-8"/>
    <property name="log.pattern"
              value="%black(%contextName-) %red(%d{yyyy-MM-dd HH:mm:ss}) %green([%thread]) %highlight(%-5level) %boldMagenta(%logger{36}) - %gray(%msg%n)"/>
    <!--输出到控制台-->
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${log.pattern}</pattern>
            <charset>${log.charset}</charset>
        </encoder>
    </appender>

    <!--每天生成日志文件 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${LOG_HOME}/server.log.%d{yyyy-MM-dd-HH}</FileNamePattern>
            <MaxHistory>60</MaxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>[%level] %d{yyyy-MM-dd HH:mm:ss.SSS} %X{logId} %c.java - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
    </appender>


    <!--普通日志输出到控制台-->
    <root level="info">
        <!-- root将级别为“INFO”及大于“INFO”的日志信息交给已经配置好的名为“Console”的appender处理，“Console”appender将信息打印到Console； -->
        <appender-ref ref="console"/>
        <!-- 标识这个appender将会添加到这个logger -->
        <appender-ref ref="FILE"/>
    </root>

    <!--    监控sql日志输出-->
    <!--  如想看到表格数据，将OFF改为INFO  -->
    <logger name="jdbc.resultsettable" level="INFO" additivity="false">
        <appender-ref ref="console"/>
    </logger>

    <!--    包含 SQL 语句实际的执行时间 及sql语句（与jdbc.sqlonly功能重复）   -->
    <logger name="jdbc.sqltiming" level="INFO" additivity="false">
        <appender-ref ref="console"/>
    </logger>

    <!--      仅仅记录 SQL 语句，会将占位符替换为实际的参数-->
    <logger name="jdbc.sqlonly" level="OFF" additivity="false">
        <appender-ref ref="console"/>
    </logger>

    <!--  包含 ResultSet 的信息，输出篇幅较长  -->
    <logger name="jdbc.resultset" level="ERROR" additivity="false">
        <appender-ref ref="console"/>
    </logger>

    <!-- 输出了 Connection 的 open、close 等信息  -->
    <logger name="jdbc.connection" level="OFF" additivity="false">
        <appender-ref ref="console"/>
    </logger>

    <!--    除了 ResultSet 之外的所有JDBC调用信息，篇幅较长 -->
    <logger name="jdbc.audit" level="OFF" additivity="false">
        <appender-ref ref="console"/>
    </logger>

</configuration>

```





## 开始测试

重新运行同一个查询语句后，我们惊喜的发现，打印出来的sql语句后面有我们传进入的参数值，同时还给打印出结果集。这下就舒服多了！

```sql
2020-02-29 23:15:57.895  INFO 25016 --- [           main] jdbc.sqlonly                             : select user0_.id as id1_1_0_, user0_.age as age2_1_0_, user0_.dept_id as dept_id4_1_0_, user0_.user_name 
as user_nam3_1_0_, dept1_.id as id1_0_1_, dept1_.dept_name as dept_nam2_0_1_ from sys_user 
user0_ left outer join sys_dept dept1_ on user0_.dept_id=dept1_.id where user0_.id=1 

2020-02-29 23:15:57.925  INFO 25016 --- [           main] jdbc.resultsettable                      : 
|---------|----|--------|----------|---|----------|
|id       |age |dept_id |user_name |id |dept_name |
|---------|----|--------|----------|---|----------|
|[unread] |21  |1       |张三       |1  |财务部    |
|---------|----|--------|----------|---|----------|
```

