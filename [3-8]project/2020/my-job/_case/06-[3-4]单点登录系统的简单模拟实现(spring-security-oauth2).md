# [3-4]单点登录系统的简单模拟实现(spring-security-oauth2)

> @todo

- @code https://github.com/service-java/summer-demo-sso-20210425

```
首先不幸的消息是 spring-security-oauth2是处在Deprecated状态下的社区项目, 但好在它还在维护中

接盘的spring-authorization-server好像还没啥人用??

比较高端的有apereo-cas

其次按照知名度，我想到了xxl-sso(貌似不维护了)和kisso

比较新的有MaxKey和sa-token
```

# 单点登录

- 单点登录（SSO）
  - https://juejin.cn/post/6912605392799268877

```js
// 获取 token
var token = result.data.token;

// 动态创建一个不可见的iframe，在iframe中加载一个跨域HTML
var iframe = document.createElement("iframe");
iframe.src = "http://app1.com/localstorage.html";
document.body.append(iframe);
// 使用postMessage()方法将token传递给iframe
setTimeout(function () {
    iframe.contentWindow.postMessage(token, "http://app1.com");
}, 4000);
setTimeout(function () {
    iframe.remove();
}, 6000);

// 在这个iframe所加载的HTML中绑定一个事件监听器，当事件被触发时，把接收到的token数据写入localStorage
window.addEventListener('message', function (event) {
    localStorage.setItem('token', event.data)
}, false);
```

---

# spring-security-oauth2

# spring-security-oauth2-实践

- Spring Security OAuth2 单点登录和登出 @nice
  - @by https://github.com/shpunishment  
  - @code https://github.com/shpunishment/spring-security-oauth2-demo
  - @doc https://blog.csdn.net/qq_36160730/article/details/105909308

```
http://localhost:8001/service1/
http://localhost:8002/service2/
```

- Spring Security OAuth2 token存储Redis用户登出logOut
  - @code https://github.com/mingyang66/spring-parent/tree/master/spring-security-oauth2-server-redis-service
  - @doc https://blog.csdn.net/yaomingyang/article/details/97284851

- 基于Spring Security Oauth2的SSO单点登录+JWT权限控制实践 
    - @by https://github.com/hansonwang99 		
	- @doc https://blog.csdn.net/wangshuaiwsws95/article/details/89913614
	- @code https://github.com/hansonwang99/Spring-Boot-In-Action/tree/master/springbt_sso_jwt 	

```
spring-cloud-starter-oauth2
```

- SpringSecurity+OAuth2实现单点登录SSO（详细教程+源码）
    - @by http://www.eknown.cn/   
    - @code https://github.com/laolunsi/spring-boot-examples/tree/master/13-sso-oauth2-demo	
    - @doc 
      - https://blog.csdn.net/qq_28379809/article/details/102734384
      - https://www.baeldung.com/sso-spring-security-oauth2


```
spring-security-oauth2-autoconfigure

thymeleaf-extras-springsecurity5

@EnableOAuth2Sso
```

- 基于Spring Security + OAuth2 的SSO单点登录（客户端+服务端）
  - @doc
    - https://blog.csdn.net/qq_34997906/article/details/97007709
    - https://blog.csdn.net/qq_34997906/article/details/100014815
  - @code 
    - https://github.com/Janche/spring-security-oauth2-sso 	
    - https://github.com/Janche/sso-oauth2-server
    - https://github.com/Janche/sso-oauth2-client

- 单点登录SSO解决方案之SpringSecurity+JWT实现
  - @code https://github.com/Oxygen404/Springsecurity 
  - @doc https://blog.csdn.net/qq_38526573/article/details/103409430

- springboot+springsecurity单点登录sso实现（csrf过滤器post验证）
  - @code https://gitee.com/zhangchai/ssodemo 
  - @doc https://blog.csdn.net/weixin_40773253/article/details/84589785

# spring-security-oauth2-项目 @other

- SpringCloud+SpringBoot+OAuth2+Spring Security+Redis 一 认证授权服务（微服务）
  - Zuul网关
  - https://blog.csdn.net/xyjcfucdi128/article/details/87163388
  - https://blog.csdn.net/Amor_Leo/article/details/101751690
  - https://github.com/SophiaLeo/sophia_scaffolding
  - https://codechina.csdn.net/mirrors/sophialeo/sophia_scaffolding
  - https://github.com/2224132867/SpringCloud-OAuth2-Spring-Security-Redis

- Spring Security OAuth2.0实现单点登录SSO @old
  - @by https://haoxiaoyong.cn/2020/03/18/2020/2020-3-18-hxstrix/
  - @code https://github.com/haoxiaoyong1014/spring-security-sso
  - @doc https://blog.csdn.net/haoxiaoyong1014/article/details/80107550


- Spring Security基于JWT实现SSO单点登录  
  - https://blog.csdn.net/qq_36144258/article/details/79425942
  - https://github.com/longfeizheng/sso-merryyou

```
spring-security-oauth2
spring-security-jwt
```

- awesome-admin @old
  - 自定义SSO统一登录页面
  - https://gitee.com/awesome-engineer/awesome-admin

---

# spring-authorization-server

- @code https://github.com/spring-projects-experimental/spring-authorization-server

---

# xxl-sso

- @code 
  - https://github.com/xuxueli/xxl-sso/issues
  - https://github.com/service-java/xxl-demo-sso-20210423

---

# Sa-Token

- @doc http://sa-token.dev33.cn/doc/index.html#/senior/sso
- @code 
  - https://gitee.com/click33/sa-plus
  - https://gitee.com/dcy421/dcy-fast
  - https://gitee.com/wtsoftware/jthink

```
跨域模式下的解决方案
 
转投LocalStorage的怀抱
```

# Sa-Token @faq

- 一个User对象存进Session后，再取出来时报错：无法从User类型转换成User类型

```
关闭热加载
```

---

# 参考 @ref






