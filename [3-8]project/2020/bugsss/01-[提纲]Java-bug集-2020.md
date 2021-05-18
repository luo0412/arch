# [提纲]Java-Bug集-2020



# 2020-11

- tomcat捕捉了上传超过最大限制的异常 @todo

```java
@Configuration
public class TomcatConfig {

    @Bean
    public EmbeddedServletContainerFactory servletContainer() {
        TomcatEmbeddedServletContainerFactory tomcatServletContainerFactory = new TomcatEmbeddedServletContainerFactory();

        // 对文件上传大小限制异常交还控制器处理
        // 不然tomcat会处理掉异常无法返回给前端
        tomcatServletContainerFactory.addConnectorCustomizers((TomcatConnectorCustomizer) connector -> {
            if ((connector.getProtocolHandler() instanceof AbstractHttp11Protocol<?>)) {
                // -1 means unlimited
                ((AbstractHttp11Protocol<?>) connector.getProtocolHandler()).setMaxSwallowSize(-1);
            }
        });

        return tomcatServletContainerFactory;
    }
}
```

# 2020-10

- java.lang.NoClassDefFoundError: org/apache/poi/util/TempFileCreationStrategy
    - https://github.com/alibaba/easyexcel/issues/1497
    - https://github.com/alibaba/easyexcel/issues/112

```
建议升级到3.17解决

===
如果是jdk>=1.8的话可以替换成poi4.0.0即可解决，有可能是poi的bug导致的

  <dependency>
  	<groupId>org.apache.poi</groupId>
  	<artifactId>poi-ooxml</artifactId>
  	<version>4.0.0</version>
  </dependency>
  <dependency>
  	<groupId>org.apache.poi</groupId>
  	<artifactId>poi</artifactId>
  	<version>4.0.0</version>
  </dependency>
  <dependency>
  	<groupId>org.apache.commons</groupId>
  	<artifactId>commons-compress</artifactId>
  	<version>1.18</version>
  </dependency>

===
maven-shade-plugin
```

# 2020-09

- java.sql.SQLException: Value ‘0000-00-00 00:00:00’ can not be represented as java.sql.Timestamp
  - https://blog.csdn.net/zhuzj12345/article/details/84333672

# 2020-08

- CVE-2020-24616: Jackson 多个反序列化安全漏洞通告
  - https://mp.weixin.qq.com/s/13W3egANrLvT5LhoD6u-6A

```java
升级到 jackson-databind 2.9.10.6
```


# 2020-07

- mysql驱动包版本

```java
com.mysql.jdbc.Driver 
com.mysql.cj.jdbc.Driver
```

# 2020-06

- `Dubbo高危反序列化漏洞`，存在远程代码执行风险，建议及时升级到2.7.7或更高版本！
    - https://mp.weixin.qq.com/s/pQEnJ4M5YVlwpY2igl4rbA

```java
攻击者可以通过RPC请求发送无法识别的服务名称或方法名称以及一些恶意参数有效载荷
当恶意参数被反序列化时
可以造成远程代码执行

升级到2.7.7或更高版本
```

- Required request body is missing
  - https://blog.csdn.net/qq_40929531/article/details/88559563
  - https://blog.csdn.net/q1035331653/article/details/80370818

```java
如果是get方法, 可以直接不加 @RequestBody

@RequestBody(required=false) 虽然可以放过但还是需要判空逻辑
```

- IDEA Error:java: Compilation failed: internal java compiler error
  - https://www.cnblogs.com/comeluder/p/8215317.html

```java
File-->Setting...-->Build,Execution,Deployment-->Compiler-->Java Compiler 
设置相应Module的target bytecode version的合适版本
```

- `Fastjson最新高危漏洞`来袭 @attention
    - https://mp.weixin.qq.com/s/bzCrKP3uzEEUWHQyP5tdYg
    - https://mp.weixin.qq.com/s/BTOKVBB_eH-e2avf8yqC4w

```java
// 方案
1) 升级Fastjson版本到1.2.68 --> 1.2.69!!!
2) ParserConfig.getGlobalInstance().setSafeMode(true)

// 描述
Fastjson存在远程代码执行漏洞
autotype开关的限制可以被绕过
链式的反序列化攻击者精心构造反序列化利用链
最终达成远程命令执行的后果
```


- 非法关机后`mysql.sock`文件

```bash
rm mysql.sock
service mysqld restart
```

# 2020-05

- tomcat---Request header is too large和Request Entity Too Large的正确解决方法
  - https://blog.csdn.net/lianjunzongsiling/article/details/79902938

```xml
<!--设置maxPostSize限制为200MB，设置maxHttpHeaderSize限制为16KB-->
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" 
    maxPostSize="209715200" maxHttpHeaderSize="16384"/>
```

- `CentOS环境Tomcat配置JDK`的另一种方式
  - https://www.cnblogs.com/zhi-leaf/p/11427187.html

```bash
# touch bin/setenv.sh

#!/bin/sh

export JAVA_HOME=/usr/local/jdk1.8.0_191/
```

- jar no main manifest attribute --> spring-boot-maven-plugin

- XML无效字符

```java
0x00 - 0x08  
0x0b - 0x0c  
0x0e - 0x1f
```

# 2020-04-28

> 一些没什么太大影响的报错??

- javax.management.InstanceNotFoundException: org.springframework.boot:type=Admin,name=SpringApplication @ignore

- org.apache.shiro.session.UnknownSessionException: There is no session with id @ignore

- java.lang.ClassNotFoundException: java.lang.FunctionalInterface @ignore

- org.apache.shiro.session.UnknownSessionException?? @todo

# 2020-04

- org.springframework.orm.jpa.vendor.HibernateJpaDialect.`convertHibernateAccessException`
  - https://blog.csdn.net/boling_cavalry/article/details/79342572

- idea.cycle.buffer.size --> idea配置日志行数

- java.nio.channels.`ClosedChannelException`
  - 表格先读一遍校验格式, 然后异步解析内容(大数据量) --> 直接把对象放在异步函数里当参数传下去
  - closeQuietly() 与直接close() ??
  - https://www.jianshu.com/p/4edd1775b983

```java
@Async
public UploadProcessStatus handleParseExcel(final InputStream is) {} // final限制去掉??
```

- `循环依赖` --> consider using 'getBeanNamesOfType' with the 'allowEagerInit' flag turned off

```java
手抖, 去掉错误依赖的模块
```


- JPA调用repository.save()保存失败 405

```java
排序字段直接顺手叫order 
--> 关键字啊...又给忘了这茬...
改叫list_order
```

- JPA某个字段一直无法更新??

```java
// updatable指定了false
@Column(name = "PERIODICAL_CODE", nullable = false, length=64 , updatable = false)
private String periodicalCode;
```

- org.hibernate.exception.`ConstraintViolationException`: could not execute statement
- org.springframework.dao.`InvalidDataAccessApiUsageException`

```java
FrameArea findByCodeAndStatus(String code, String status);
--> FrameArea findByCodeAndStatus(String code, Status status);
```

- java.lang.`UnsatisfiedLinkError` --> SQLite环境的问题?? @todo
- `Calender获得的月份`应该 +1 才对
- `org.springframework.jdbc.UncategorizedSQLException`：异常
    - 测试数据库是直接拷贝旧系统的, 那个角色没有权限...
    - `The user specified as a definer` --> 数据库权限问题


- 使用Spring提供的`BeanUtils.copyProperties`()方法报错：Could not copy property 'xxx' from source to target
  - `java.lang.reflect.InvocationTargetException`
  - https://www.cnblogs.com/exmyth/p/12488347.html

```java
// 最终原因...
javaBean的属性类型使用包装类型，不要使用基本类型
```

- JDK8 stream toMap() java.lang.IllegalStateException: Duplicate key异常解决(key重复)
  - https://www.cnblogs.com/han-1034683568/p/8624447.html

```java
// 遇到key重复, 处理一下
Map<Long, Entity> entityMap = entityList.stream().collect(Collectors.toMap(Entity::getType, Function.identity(), (entity1,entity2) -> entity1));
```

- MySQL`表损坏`预防与修复 @digest @todo
  - https://www.cnblogs.com/huzi007/p/4670505.html

---

# 2020-03

- web.xml is missing and failOnMissingWebXml is set to true @ignore

```xml
<!-- pom.xml的war报错解决方法???? 貌似没用 -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-war-plugin</artifactId>
    <version>2.3</version>
    <configuration>
        <failOnMissingWebXml>false</failOnMissingWebXml>
    </configuration>
</plugin>
```

# 2020-02

- How to avoid the `Circular view path` exception with Spring MVC test --> 未解之谜, 同事重启就可以

```java
// 最终原因
原工程目录结构不规范
idea 对应webapp目录 mark as resources
```

# 2020-01-17

- mysql重启失败 --> 配置文件ini写错(linux上的大小写敏感配置)

```
lower_case_table_names=1
```

# 2020-01

- idea跑项目`run可以跑起来，debug起不来` --> 先取消断点
  - https://blog.csdn.net/xiaobudingCC/article/details/78834399

```java
断点 --> 启动慢 --> 跑不起来???

idea项目里面的breakpoints过多引起发的，把不需要的删掉
```

- jdk.management.resource.internal.inst不存在

```java
可能jdk版本有关

删除无关import
```

# 参考