# Springboot入门

## 1、什么是SpringBoot

答：Springboot是pivotal团队开发的全新框架，是基于Spring基础上的一套解决方案，**作用是简化Spring项目的搭建和开发过程**。

## 2、SSM不方便的地方

1、war包放到tomcat里面才能运行

2、配置文件不方便

3、导jar不方便

## 3、为什么要学习SpringBoot?

### 1、独立运行

​	SpringBoot内置各种Servlet容器，如Tomcat，Jetty等，使得应用可以打包成一个Jar包独立运行。

### 2、自动配置

​	Springboot能够自动配置Spring应用程序,**除了数据库以外,习惯优于配置**。

### 3、简化开发

​	Springboot集成了大量的第三方库配置，使得这些库在SpringBoot中可以零配置使用。



## 4、SpringBoot配置文件的三种

1. application.properties
2. application.yaml
3. application.yml

## 5、常见的对象实体类型

Bean对象 数据库映射

VO对象 用于获取前端的请求数据和参数校验。

DTO 用于给前端响应数据，隐藏敏感内容。





# Lombok插件

作用是用来生成set、get、构造函数、序列化等常见代码

1、导包

```xml
<!--lombok插件-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
</dependency>
```

2、下一个插件

![image-20240305203524196](Springboot入门.assets/image-20240305203524196.png)





# 工程的创建

## NewProject

Group: 公司域名反写

Artifact: 项目名（子父工程）

Type: 项目构建方法

Language 开发语言

Packaging 打包方式

Java Version 开发版本

SpringBootVersion SpringBoot版本

Name：项目名（单纯项目名）

Description：项目介绍

Package：包路径



java -jar 名称.jarr



# 常用命令和快捷键

1. cd 跳转命令
2. dir 显示当前位置的内容
3. Ctrl + 鼠标左键进入对象
4. Ctrl + Q 返回













# 项目流程

**1、整理需求（产品经理）** 写PPT

**2、概要设计（项目经理）**写文档

**3、修Bug做项目架构 （技术经理 CTO）** 

3、出效果图（UI设计）

4、详细设计

...

5、写页面（前端工程师）

**6、写接口，出接口文档（后端工程师）**

--------------------------------------------------------------开发不能碰生产环境数据库-----------------------------------------------------------------------------

--------------------------------------------------------------运维和实施不能碰代码-----------------------------------------------------------------------------------

**7、项目部署（实施工程师）**

**8、项目维护（运维工程师）**





# knife4j 接口文档框架

## 使用方式

### 1、导包(springboot2)

```xml
<!--接口文档-->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-openapi2-spring-boot-starter</artifactId>
    <version>4.4.0</version>
</dependency>
```

### 2、配置接口文档

```yaml
# 接口文档配置
# Knife4j的配置开始
knife4j:
  enable: true                       # 启用Knife4j，true表示启用，false表示禁用
  openapi:
    title: SpringBoot学习项目           # 设置OpenAPI文档的标题为"Knife4j官方文档"
    description: "`我是测试`,**你知道吗**  
    # aaa"                           # 设置OpenAPI文档的描述。
    email: 123@qq.com      # 设置OpenAPI文档的联系人邮箱
    concat: Z21软件后端开发团队         # 设置OpenAPI文档的附加信息为
    url: https://docs.xiaominfo.com  # 设置OpenAPI文档的URL为
    version: v4.0                    # 设置OpenAPI文档的版本为"v4.0"
    license: Apache 2.0              # 设置OpenAPI文档的许可为"Apache 2.0"
    license-url: https://stackoverflow.com/  # 设置许可的URL为
    terms-of-service-url: https://stackoverflow.com/  # 设置服务条款的URL为
    group:                           # 开始定义API分组
      test1:                         # 第一个API分组名为"test1"
        group-name: 分组名称         # 设置该分组的名称为“分组名称”
        api-rule: package            # 设置该分组的规则为“package”，通常表示根据包名进行API分组
        api-rule-resources:          # 设置满足上述规则的包路径列表
          - com.example.demo.controller   # 第一个包路径为
```

### 3、访问文档地址

**http://ip地址:端口号/doc.html**

### 4、常用注解

```
@Api(tags = "登录接口")  用来标注Controller类
```

```
@ApiOperation(value = "用户名密码登录") 用来标注方法
```

```
@ApiModelProperty(value = "用户名",required = true) 用来标注字段
```



# 异常拦截

拦截类上面加 @ControllerAdvice

拦截方法上面加 @ExceptionHandler(异常类.class)

## 参数校验

```java
@ExceptionHandler(value = {BindException.class, ValidationException.class, MethodArgumentNotValidException.class})
public ResponseEntity<Result<String>> handleValidatedException(Exception e) {

    Result<String>  result = null;
    if (e instanceof  MethodArgumentNotValidException) {
        // 强制类型转换
        MethodArgumentNotValidException ex =(MethodArgumentNotValidException)  e;
        result = Result.error(86202,"参数校验异常",
                ex.getBindingResult().getAllErrors().stream()
                        .map(ObjectError::getDefaultMessage)
                        .collect(Collectors.toList())
        );
    } else  if (e instanceof ConstraintViolationException){
        ConstraintViolationException ex = (ConstraintViolationException) e;
        result = Result.error(86202,"参数校验异常",
                ex.getConstraintViolations().stream()
                        .map(ConstraintViolation::getMessage)
                        .collect(Collectors.toList())
        );
    }else  if (e instanceof BindException) {
        BindException ex = (BindException ) e;
        result = Result.error(86202,"参数校验异常",
                ex.getAllErrors().stream()
                        .map(ObjectError::getDefaultMessage)
                        .collect(Collectors.toList())
        );
    }
    return new ResponseEntity<>(result,HttpStatus.BAD_REQUEST);
}
```



# 用户注册


# 加密方式
## 对称加密（私钥加密、共享密钥加密）
常见的算法有：DES、3DES、IDEA、RC4、RC5、RC6、AES等
公钥：公钥用于加密
私钥：私钥用于解密
## 非对称加密
常见的算法有：RSA、ECC、DSA等
## 不可逆加密(摘要加密)
常见算法： MD5、SHA系列等，这些算法可以将明文转化为固定长度的密文，但无法通过密文还原出明文，因此是不可逆的。


# 热部署
1、导包
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>3.2.0</version>
</dependency>
```

2、配置项目的自动构建

![image-20240312185209048](README.assets/image-20240312185209048.png)

3、Ctrl + Shift + Alt + /

![image-20240312185311177](README.assets/image-20240312185311177.png)

4、c-a-a-w-a-r 勾上

compiler.automake.allow.when.app.running 勾上

4、还有一地方

![image-20240312185632686](README.assets/image-20240312185632686.png)





# 项目的生命周期

## 1、开发环境 dev

程序员自己的电脑

## 2、测试环境 test

模拟的生产环境，用于上线前的测试

## 3、生产环境 prod

就是线上有用户使用的环境

## 语法规则

在springboot中我们可以创建 application-{环境名}.yml/properties文件来代表不同环境下的配置。

![image-20240312192042049](README.assets/image-20240312192042049.png)

在主配置文件中使用active来切换不同的环境

```yml
spring:
  profiles:
    active: test # 当前整个项目的环境dev test prod
```



# Mybatis Plus逆向工程



添加操作

```java
//一个返回的是影响行数
getBaseMapper().insert(当前表的Entity);
//一个返回的是当前数据的id
this.save(当前表的Entity);
```

简单的查询操作

```java
//查询条件
QueryWrapper<UsersInfo> 查询条件 = new QueryWrapper<UsersInfo>().eq("phone", usersInfo.getPhone());
//判断手机号是否存在
UsersInfo usersInfo1 = this.getOne(查询条件);
//和上面是一样的，比上面长一点
getBaseMapper().selectOne(queryWrapper);
```





# Sa-Token

## 登录流程

在sa-token里保存用户的id

```java
StpUtil.login(login.getUserId());
```

## 获取Token

底层使用的jwt的封装

```java
//获取当前会话的token
String tokenValue = StpUtil.getTokenValue();
```

## 全局拦截器

```java
@Configuration
public class SaTokenConfigure implements WebMvcConfigurer {
     // 注册 Sa-Token 拦截器，打开注解式鉴权功能
     @Override
     public void addInterceptors(InterceptorRegistry registry) {
        // 注册 Sa-Token 拦截器，打开注解式鉴权功能
        // 拦截所有请求/**
        registry.addInterceptor(new SaInterceptor()).addPathPatterns("/**");
     }
}
```

## 认证鉴权

### 配置接口实现

```java
@Component
public class StpInterfaceImpl  implements StpInterface {

    @Autowired
    private IRoleService iRoleService;

    /**
     * 获取一个list作为权限认证
     */
    @Override
    public List<String> getPermissionList(Object loginId, String loginType) {
        // 本 list 仅做模拟，实际项目中要根据具体业务逻辑来查询权限
        List<String> list = new ArrayList<String>();
        return list;
    }

    /**
     * 返回一个账号所拥有的角色标识集合 (权限与角色可分开校验)
     * loginId登录这个人的id
     */
    @Override
    public List<String> getRoleList(Object loginId, String loginType) {
        List<String> strings = iRoleService.selectUserRoleById(Integer.parseInt(loginId.toString()));
        return strings;
    }
}
```

### 使用角色注解

```
@SaCheckRole("角色")
@SaCheckRole(value = {"角色1","角色2"},mode = SaMode.AND)
@SaCheckRole(value = {"角色1","角色2"},mode = SaMode.OR)
```

### 使用权限注解

**注解的key 一定是  模块.功能.功能**    如：user.update.age

```
@SaCheckPermission("tuition.update.money") 学费.修改.金额
```





# WebSocket协议（长连接）

协议就是一种规范，USB接口（协议），Type-C接口（协议）	

WebSocket是基于TCP的一种**网络协议**，它实现了浏览器与服务器的全双工通信，浏览器和服务器只需要完成**一次握手**，两者之间就可以创建**持久**的连接，并进行**双向**通信。

## HTTP和WebSocket协议的区别

HTTP是**短连接**而WebSocket是**长连接**

HTTP是**单向的**而WebSocket是**双向的**



## 1代码实现-导包

```xml
  <!--websocket的包-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-websocket</artifactId>
  </dependency>
```

## 2写服务端

```java
package com.example.demo.socket;

import org.springframework.stereotype.Component;

import javax.websocket.*;
import javax.websocket.server.ServerEndpoint;

/*
 *
 *   @Author: Sjy
 *   @Date: 2024/3/25-03-25-19:05
 *   用于建立长连接的类
 */
@Component
//定义此类是WS的服务器端
@ServerEndpoint("/private/chat/{userId}")
public class WebSocketServer {
    //ws一共有四种状态

    /**
     * 连接建立成功时触发
     * @param session 当前会话
     */
    @OnOpen
    public void onOpen(Session session){
        System.out.println("我成功的建立了连接");
    }
    /**
     * 连接关闭时触发
     */
    @OnClose
    public void onClose(){

    }
    /**
     * 收到客户端消息时触发
     * @param message
     */
    @OnMessage
    public void onMessage(String message){

    }
    /**
     * 发生错误时触发
     * @param error
     */
    @OnError
    public void onError(Throwable error){

    }
}

```

## 3写客户端

```javascript
//专门用来写长连接
function loginSuccess(){
    var userId = localStorage.getItem("userId");
    //如果为空就不连
    if(userId == null){
        return;
    }
    //WebSocket对象
    var ws = new WebSocket("ws://192.168.1.16:80/private/chat/"+userId);

    //连接成功时
    ws.onopen = function(){
        console.log("连接成功");
    }
    //连接关闭时
    ws.onclose = function(){
        console.log("连接关闭");
        setTimeout(()=>{
            loginSuccess();
        },2000);
    }
    //收到服务器的信息
    ws.onmessage = function(e){
        console.log(e);
    }
    //连接异常
    ws.onerror = function(e){
        console.log(e);
    }
}
//去触发长连接
loginSuccess();
```

## 4写配置类，注入WS

```java
@Configuration
public class WebSocketConfig {

    @Bean
    public ServerEndpointExporter serverEndpointExporter(){
        return new ServerEndpointExporter();
    }
}

```

![image-20240326203344456](README.assets/image-20240326203344456.png)





# Redis

## 简介

Redis是（Remote Dictionarty Server）远程字典服务，是一个开源的**NoSQL数据库**，使用ANSI C语言编写的，它是一个**基于内存**的**Key-Value**数据库，并且支持**持久化**操作。

有丰富的数据结构如：

1. String （字符串）
2. Hash    (可以理解为对象)
3. List      (双向链表)
4. Set  (string类型的无序集合)
5. Sorted Set （有序集合）



## SQL数据库

非关系型数据库，放弃了统一的技术标准（sql），为某一特定领域场景而设计，使性能、容量、扩展性都实现一定的突破。

特点

1. 不支持ACID
2. 不遵循SQL标准
3. 远超SQL性能
4. 海量数据读写
5. 高并发读写

常见NoSQL

1. mongoDB  高性能、开源、文档型数据库 
2. HBase   HBase支持数十亿行*数百万列的数据库表 是Hadoop项目的数据库



## 语法

key相关

```
nil 的意思是无值或不存在，Go、Lua、Object-C 为空都是nil
EX 20 设置键的过期时间是20秒
PX 3000 设置键的过期时间是3000毫秒 = 3秒
NX 如果键不存在则设置新的值，否则操作失败
XX 如果键已经存在就设置新的值，否则操作失败
NX和XX不能同时出现，因为他们语义互斥
```

```
在key中可以使用：（冒号）来做单词分离，如：user:1  或 user:2 user称为命名空间
keys 后面加key的名字可以用*来做模糊查询 如：keys na*e:*
del key 可以删除一个key或多个key (危险操作)
exists key 查询key是否存在（0不存在 1存在）
type key  可以返回这个key的类型
rename key newkey 给key进行重命名
expire key 秒  给指定key设置一个过期时间
pexpire key 毫秒 给指定的key设置一个毫秒过期时间
pexpireat key 时间戳 设置一个到达指定时间过期的key 用时间戳（如：1714741026000）
ttl key 获取这个key的过期时间   如果为-1永不过期   如果-2没有这个key

```





### 1、字符串语法

```
set key value 设置一个值
get key  获取值
append key 在字符串后拼接字符串
getrange key start end 截取字符串从start(包前) end结束，end可以是固定位置也可以写-1
setnx key value 但key不存在时再设置key的值
strlen key  获取value的长度
```

### 2、Hash语法

Hash在Redis中是一个广泛使用的概念，主要是指一种任意长度的数据（如字符串）通过Hash算法变换成固定长度的数据格式的过程，这个固定长度的数据通常被称为哈希值或哈希码。

**Redis中每个hash可以存储2^32-1个键值对（40多亿）**

```
hset 对象名:对象id key value key value key value key value   设置一个对象
hget 对象名:对象id 属性key 查询单个属性
hgetall 对象名:对象id     查询所有属性
hdel 对象名:对象id 属性名  删除对应属性
hexists 对象名:对象id 属性名 检查属性是否存在，存在返回1，否则返回0
hmget 对象名:对象id 字段名1 字段名2 获取指定字段的值
```

### 3、List

List在Redis中提供一种线性数据库结构，它可以存储一个字符串列表，按照插入顺序排序，可以中列表的两端推出或弹出元素。

主要特点：**元素有序**、**支持重复元素**、**可以通过索引访问元素**、**操作复杂度低**（ 头部和尾部的时间复杂度为O(1) ）

常见八股文： 如何使用List实现一个消息队列？  如何遍历Redis的List?  Redis的List最大存储多少个元素？

一个列表最多可以包含 2^32 - 1 个元素 (4294967295, 每个列表超过40亿个元素)

![image-20240402205648750](README.assets/image-20240402205648750.png)

```
lpush key value value value value value 将一个或多个值插入到列表的头部
rpush 从右边（尾部）插入
rpop key 从右边（尾部）移除并获取一个元素
lpop key 从左边（头部）移除并获取一个元素
llen key 获取链表的长度
lrange key 0 -1 获取整个链表所有的数据
lindex key index 获取指定位置的元素的数据
```

### 4、Set

Redis的Set数据结构能否能否重复？  Redis集合最大存储？2^32-1  Set和List的区别？

使用场景：随机抽奖、唯一性检查

```
sadd key value value 给set数据机构添加内容
smembers key 获取所有的元素
spop key 随机弹出并移除一个元素

```



### 5、sorted set

使用场景：实时排行榜系统，带权重的任务队列

```
zadd vips 权重 值 权重 值 权重 值 添加数据到有序集合里
zrevrange key 0 -1 权重从大到小获取所有元素 加withscores和权重一起返回
zrange key 0 -1 权重从小到大排序
```





## 安装使用

https://redis.io/



## SpringBoot整合Redis

#### 导包

```xml
<!--redis-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!--Lettuce 链接池-->
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>6.3.2.RELEASE</version>
</dependency>
```

#### Lettuce特点：

1. **高性能** Lettuce通过使用NIO（非阻塞IO）和连接池等技术，实现了高性能的数据传输与链接管理
2. **异步和响应式**  单个线程中处理多个并发请求，提高的吞吐量
3. **可靠性** 自动重连故障转移
4. **可扩展性** 支持连接池和集群模式

#### 配置

```java
@Configuration
public class RedisConfig {
    //配置序列化器 redisTemplate
    @Bean
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate<String,Object> redisTemplate=new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory);

        //设置key的序列化器
        redisTemplate.setKeySerializer(new StringRedisSerializer());

        //设置value的序列化器
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());

        return redisTemplate;
    }
}
```

```yaml
spring:
  redis:
    host: 127.0.0.1
    port: 6379
    #password: <PASSWORD>
    database: 0
    lettuce:
      pool: # 连接池
        max-active: 8 #最大活跃数
        max-wait: -1ms #但链接耗尽时，获取链接的最大等待时间
        max-idle: 8 #最大空闲链接数
        min-idle: 0 #最小空闲链接
```













![image-20240401185701197](README.assets/image-20240401185701197.png)

![image-20240401191300634](README.assets/image-20240401191300634.png)

![image-20240401204106638](README.assets/image-20240401204106638.png)

![image-20240408184141619](README.assets/image-20240408184141619.png)



# 日志输出

### 在类上添加注解

```
@Slf4j
```

使用

```java
//可以关
log.info("通过数据库查询");
//不能关
System.out.println("通过数据库查询");
```







# 自定义注解

## 什么是注解

注解就是一种标记

## 创建注解

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME) //生命周期
@Component//被IOC注入到容器
public @interface AutoCustomAnnotation {

}
```

```
@Target：指定被修饰的Annotation可以放置的位置(被修饰的目标)
@Target(ElementType.TYPE)                      //接口、类
@Target(ElementType.FIELD)                     //属性
@Target(ElementType.METHOD)                    //方法
@Target(ElementType.PARAMETER)                 //方法参数
@Target(ElementType.CONSTRUCTOR)               //构造函数
@Target(ElementType.LOCAL_VARIABLE)            //局部变量
@Target(ElementType.ANNOTATION_TYPE)           //注解
@Target(ElementType.PACKAGE)                   //包
```

```java
RetentionPolicy.SOURCE：注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃；
RetentionPolicy.CLASS：注解被保留到class文件，但jvm加载class文件时候被遗弃，这是默认的生命周期；
RetentionPolicy.RUNTIME：注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在；
```

## AOP + 注解

### 什么是AOP

面向切面编程，通过切点表达式去对方法进行增强。

### 如何找到自定义注解

```java
@Slf4j //日志注解
@Aspect //标记是一个aop切面
@Component //放到IOC容器
public class RedisCachingAspect {

    
    @Around("@annotation(com.example.demo.anno.AutoCustomAnnotation)")//可以写全路径
    public Object cacheAroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
        log.info("我被执行了");

        //放行
        Object proceed = joinPoint.proceed();
        return proceed;
    }


}
```



# 名词扩展

HR：人力资源主管

CTO：首席技术执行官

OPS：（Operations Per Second）每秒可以执行的运算次数（请求次数）

QPS：（Queries Per Second） 每秒查询速率

端口： 值的是计算机与外部通信的出入口，范围是0~65535







