# [0-6]导出数据库结构与API接口至Word文档的几个好用插件(screw+knife4j)

> 管理要求要导出这些文档，哎，插件找的好，整体来说还是很顺利的

- knife4j-离线文档功能
- screw导出
- navicat直接导出 @ignore @simple

---

# knife4j导出

- Failed to start bean 'documentationPluginsBootstrapper'
	- 引用了knife4j，可以先移除所有的springfox依赖
	- 项目的版本比较老，新项目最好用新版本
	- 遇上点版本问题，SpringBoot是v1.5.9.RELEASE，不知道有没有关系
	- 不要忘记在spring security里放行
	- https://github.com/springfox/springfox/issues/3282
	- https://doc.xiaominfo.com/guide/sp-nmerror.html

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.8</version>
    <exclusions>
        <exclusion>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.springframework.plugin</groupId>
            <artifactId>spring-plugin-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.springframework.plugin</groupId>
    <artifactId>spring-plugin-core</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>

<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>29.0-jre</version>
</dependency>
```

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
        //enabling swagger-ui part for visual documentation
        registry.addResourceHandler("swagger-ui.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");

}
```

```java
@Bean
public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
.apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .build()
                .securitySchemes(securitySchemes())
                .securityContexts(securityContexts());
}
```

```
@Configuration
@EnableKnife4j
@EnableSwagger2WebMvc
```

---

# Screw导出

- 直接使用Maven插件

```xml
<!-- @from https://www.iocoder.cn/Spring-Boot/DB-Doc-screw/ -->
<build>
	<plugins>
		<plugin>
			<groupId>cn.smallbun.screw</groupId>
			<artifactId>screw-maven-plugin</artifactId>
			<version>1.0.5</version>
			<dependencies>
				<!-- 数据库连接 -->
				<dependency>
					<groupId>com.zaxxer</groupId>
					<artifactId>HikariCP</artifactId>
					<version>3.4.5</version>
				</dependency>
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>${mysql-connector-java.version}</version>
				</dependency>
			</dependencies>
			<configuration>
				<!-- 数据库相关配置 -->
                <driverClassName>com.mysql.jdbc.Driver</driverClassName>
				<!-- <driverClassName>com.mysql.cj.jdbc.Driver</driverClassName> -->
				<jdbcUrl>jdbc:mysql://400-infra.server.iocoder.cn:3306/mall_system</jdbcUrl>
				<username>root</username>
				<password>3WLiVUBEwTbvAfsh</password>
				<!-- screw 配置 -->
				<fileType>WORD</fileType>
				<title>某某系统-数据库文档</title>
				<!--标题-->
				<fileName>某某系统-数据库文档</fileName>
				<!--文档名称 为空时:将采用[数据库名称-描述-版本号]作为文档名称-->
				<description>数据库文档自动生成</description>
				<!--描述-->
				<version>${project.version}</version>
				<!--版本-->
				<openOutputDir>false</openOutputDir>
				<!--打开文件输出目录-->
				<produceType>freemarker</produceType>
				<!--生成模板-->
			</configuration>
			<executions>
				<execution>
					<phase>compile</phase>
					<goals>
						<goal>run</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

# Navicat导出 @ignore

- 执行查询SQL

```sql
SELECT
  TABLE_NAME 表名,
  COLUMN_COMMENT 列名,
  COLUMN_NAME 字段代码,
  DATA_TYPE 字段类型,
  COLUMN_TYPE 字段长度,
  IF(IS_NULLABLE='NO','是','否') '空/非空',
  '' 约束条件,
  COLUMN_COMMENT 备注
  FROM
  INFORMATION_SCHEMA.COLUMNS

where
  -- 填写数据库名称
  table_schema ='idata-zhoushan-0'
  -- 可以限定表名
  -- AND table_name = "biz_qd"

order by 
	table_name
```

- 选择`导出结果`到word格式 + 需要勾选包含列的标题

---

# 参考 @ref

- 芋道 Spring Boot 数据表结构文档
  - https://www.iocoder.cn/Spring-Boot/DB-Doc-screw/
- https://blog.csdn.net/Wwt819635881/article/details/105419403
- https://blog.csdn.net/cxhtgm/article/details/81034001