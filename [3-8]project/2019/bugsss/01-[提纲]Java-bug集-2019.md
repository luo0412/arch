# [提纲]Java-Bug集-2019


# 2019-11

- idea多模块红色提示找不到本地模块jar，但是点击代码却可以直接跳转过去

```java
idea > file > Invalidate Caches 清理缓存 
重启idea
```

- postman报错: HTTP method names must be tokens

```java
https --> http
```

# 2019-10

- Invalid byte tag in constant pool: 19

```
tomcat版本有关
```

- `tomcat-UTF8编码问题`

```yml
spring:
  http:
    encoding:
      enabled: true
      force: true
      charset: UTF-8
  resources:
    static-locations: classpath:/static/
```

# 2019-09

- 对linux安装中文字体库
    - https://www.cnblogs.com/xiaochina/p/9779671.html

```bash
# 字体扩展
mkfontscale
# 新增字体目录
mkfontdir
# 刷新缓存
fc-cache -fv

# 验证 
fc-list 
```

- 引入`第三方jar包NoClassDefFoundError`
    - https://blog.csdn.net/qq_34807722/article/details/88032707

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
    	<!-- 此处配置可以使jar包通过./demo.jar的方式启动(需要给jar包添加执行权限) -->
        <executable>true</executable>
        <!-- 引入第三方jar包时必须配置 -->
        <includeSystemScope>true</includeSystemScope>
    </configuration>
</plugin>
```

- Spring boot应用启动后首次访问很慢的问题 --> `Java随机数生成依赖熵源`
    - https://blog.csdn.net/qq_33811662/article/details/81506222
    - https://blog.csdn.net/u012954933/article/details/85935730

```java
Java随机数生成依赖熵源（Entropy Source），
默认的阻塞型的 /dev/random熵源可能导致阻塞，
而换一个非阻塞的 /dev/urandom的熵源就可以了。

// $JAVA_HOME/jre/lib/security/java.security
securerandom.source=file:/dev/random
-->  securerandom.source=file:/dev/urandom
```

- mysql遇见Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggre的问题
    - https://blog.csdn.net/anaitudou/article/details/80496082

```bash
# sudo vim /etc/mysql/conf.d/mysql.cnf
[mysqld] 
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

- Tika判断文件类型（可正确判断） 
    - https://blog.csdn.net/bingguang1993/article/details/86692332

- `"Failed to decode downloaded font"`和"OTS parsing error: Failed to convert WOFF 2.0 font to SFNT"
    - https://blog.csdn.net/wochunyang/article/details/80583121
    - https://www.cnblogs.com/tomcatandjerry/p/5799188.html
    - 字体可能已经损坏 需要重新构建生成!!

```xml
<!-- 像这样写两遍?? 有点诡异 -->
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <excludes>
            <exclude>static/layui/**/*</exclude>
        </excludes>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>false</filtering>
        <includes>
            <include>static/layui/**/*</include>
        </includes>
    </resource>
</resources>
```

- idea输出 Could not find key 'SERVER_address' in any property @ignore

```java
调整日志级别?? --> 暂时无解 
不过, 并不影响正常运行, 忽略
```

- `Tomcat对于CORS跨域访问的快速支持`

```xml
<filter>
    <filter-name>CorsFilter</filter-name>
    <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
    <init-param>
        <param-name>cors.allowed.origins</param-name>
        <param-value>*</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CorsFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("*")
            .allowCredentials(true)
            .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
            // 设置一下
            // .allowedHeaders("*")
            // .exposedHeaders(
            // "access-control-allow-headers",
            // "access-control-allow-methods",
            // "access-control-allow-origin",
            // "access-control-max-age",
            // "X-Frame-Options")
            .maxAge(3600);
    }
}
```

```js
const service = axios.create({
    baseURL: url, // api 的 base_url
    timeout: 10000, // request timeout
    withCredentials: true,
})
```

# 2019-07

- redis启动报错 --> 调整jedis版本 (2.9.1)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <exclusions>
        <exclusion>
            <groupId>io.lettuce</groupId>
            <artifactId>lettuce-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

# 2019-05

- The `server time zone` value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone.
  - https://www.cnblogs.com/staring0815/p/10362525.html

```sql
mysql> set global time_zone='+8:00';
```


# 2019-04

- `jackson + lombok` -> @JsonFormat失效
  - jackson和lombok各自很强, 在一起就是残废
  - 希望升级版本后可以解决!!

```java
// 添加@JsonProperty
@JsonProperty("nDate")
@JsonFormat(pattern = "yyyy-MM-dd", timezone="GMT+8")
private Date nDate;

// ---> lombok @Data生成的是 (注意但小写)
public BizNewVo setNDate(Date nDate) {
    this.nDate = nDate;
    return this;
}

// 会诡异地多出字段!!!
ndate
nDate

所以注意命名规范与语义
```

# 2019-02

- HTTP请求中的Form Data与Request Payload的区别 
    - https://github.com/kaola-fed/blog/issues/105

```js
// Ajax全局配置
$.ajaxSetup({
    // 1. 设置contentType为json
    contentType: "json",
    contentType: "application/json; charset=utf-8"
});

// 2. data需要JSON.stringify 或者qs.stringify发送
```

- 批量删除传参问题

```java
// 1. 参数加上@RequestBody
// 2. 参数类型
long[] --> 前端request payload直接传数组
或者 有属性为数组的对象(k v) -> 前端request payload k-v传递

@RequestMapping("/delete")
public R delete(@RequestBody Long[] companyIds) {
    bizCompanyService.removeByIds(Arrays.asList(companyIds));

    return R.ok();
}

// @attention
// 后来经过测试, 请求参数用逗号连接，参数List<Integer>接受 也是可以的
// @eg  http://localhost:9527/api/deleteUserBatch?ids=340,339
public Object deleteUserBatch(@RequestBody @RequestParam("ids") List<Integer> ids, ModelMap modelMap) {
    return R.ok();
}
```


# 2019-01

- 微信保存昵称失败(带表情/emoji) -> 数据库设为`utf-8 mb4`
