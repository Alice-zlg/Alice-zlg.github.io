---
title: "微服务的相关学习笔记"
date: 2024-4-27 14:46:00
tag: java
type: "note"
---

## 工具类包-StrUtil

String字符串工具包 提供了大量的字符串相关方法

### 依赖导入

```xml
<!--StringUtils工具类 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.12.0</version>
</dependency>
```

#### 使用方法

```java
StrUtil.方法名()
```

isNotBlank

用于判断字符串是否为空，null，""，等等

## 对于不同的包的配置类如何生效

在resource目录下的 META-INF 创建spring.factories文件
早期是使用spring.factories
现在改为EnableAutoConfiguration
配置类中写对应的类路径

```yaml
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
对应类路径
```

### 多个路径使用,\隔开

### 对于不想要生效的配置类

#### 使用 @Conditional

```javas
@ConditionalOnClass()
```

对于注解@ConditionalOnClass() 参数为 类.class 没有对应类将不进行注入

## 微服务与微服务之间的数据传输

一个微服务在与另一个微服务进行数据交互时用 OpenFeign 发起的远程调用不会自动传递信息

需要自己定义拦截器，添加请求头

```java
    @Bean
    public RequestInterceptor userInfoRequestInterceptor(){
        return new RequestInterceptor() {
            public void apply(RequestTemplate template) {
                Long userId = UserContext.getUser();
                if (userId != null){
                    template.header("user-info", userId.toString());
                }
            }
        };
    }
```

#### OpenFeign的使用需要开启注解

```java
@EnableFeignClients(basePackages = "com.zlg.api.client",defaultConfiguration = DefaultFeignConfig.class)
```

## 微服务配置管理

<img src="https://img2.imgtp.com/2024/04/27/pERzUWKl.png" alt="">

### 1.添加配置到 Nacos 配置中心

<img src="https://img2.imgtp.com/2024/04/29/9VPoAAUN.png" alt="" />

对于需要动态修改的配置内容使用 "${}" 变量 的方式作为占位符,然后在需要改动的配置中进行变量的赋值

```yaml
hm:
  db:
    host: mysql
    pw: 123
```

其中 ${ :} 表示如果没有进行变量赋值时的默认值

### 2.拉取共享配置

springCloud 启动的流程

<img src="https://img2.imgtp.com/2024/04/29/uCqc3iPK.png" alt="">

1. 通过xml依赖引入，拉取配置文件和读取 bootstrap 文件

```xml

<dependencys>
    <!--nacos配置管理-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    </dependency>
    <!--读取bootstrap文件-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-bootstrap</artifactId>
    </dependency>
</dependencys>
```

2. 新建 bootstrap.yaml 配置文件 配置nacos和共享配置文件的配置

```yaml
spring:
  application:
    name: cart-service # 微服务名称
  profiles:
    active: dev
  cloud:
    nacos:
      server-addr: 192.168.150.101 # nacos地址
      config:
        file-extension: yaml # 文件后缀名
        shared-configs: # 共享配置
          - dataId: shared-jdbc.yaml # 共享配置文件id
```

## 配置热更新

通过修改nacos中的配置可以实现不重启项目实现更新项目
这边通过配置一个变量来演示

自定义的配置类和参数

```java
@Data
@Component
@ConfigurationProperties(prefix = "hm.cart")
public class CartProperties {
    private  Integer maxItems;
}
```

实际的使用场景

```java
    private void checkCartsFull(Long userId) {
        int count = lambdaQuery().eq(Cart::getUserId, userId).count();
        if (count >= cartProperties.getMaxItems()) {
            throw new BizIllegalException(StrUtil.format("用户购物车数量不能超过{}", cartProperties.getMaxItems()));
        }
    }
```

nacos 中的配置
<img src="https://img2.imgtp.com/2024/05/19/tw8pOfrW.png">
在nacos中写没有提示，建议现在idea中编写好后再进行复制
nacos 配置的命名规范
<img src="https://img2.imgtp.com/2024/05/05/vatViBTq.png" >

然后就可以实现修改nacos中的配置文件达到项目的修改-简称"热更新"

## 雪崩问题

什么是雪崩问题：见名知义，就是在为服务中由于一个微服务模块的故障导致链路中的所有服务都无法使用

### 保护方案

1. 请求限流-流量整形
   限制接口的请求并发数量，避免服务因流量激增出现故障 将原本忽高忽低的请求量限制在一定的范围上
2. 线程隔离-舱壁模式
   模拟船舱隔板的防水原理。通过限定每个业务能够使用的线程数量而将故障隔离，避免故障扩散。
3. 服务熔断
   由断路器统计请求的异常比例或调慢比例，如果超出阈值则熔断该业务，则拦截该接口的请求。 熔断期间，所有请求快速失败，全部走fallback逻辑

### 保护技术

<img src="https://img2.imgtp.com/2024/05/05/cYQ1lGBz.png" alt="">

### 技术实现

#### 1.启动 sentinel

通过阿里巴巴的 sentinel-dashboard 实现
下载相关的jar包，使用java虚拟机运行后访问
相关启动命令：

```shell
java -Dserver.port=8090 -Dcsp.sentinel.dashboard.server=localhost:8090 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```

注意: jar包的位置不能存在中文字符

启动成功后在浏览器输入对应的端口号就能进入登录页面

<img src="https://img2.imgtp.com/2024/05/06/z6yUPpwP.png" alt="">

用户名和密码都是 sentinel
进入控制台后就可以通过控制台实现对项目相关的各种控制了

#### 2. 联系项目

导入sentinel依赖

```xml
<!--sentinel-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

配置控制台

```yaml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8090
```

然后访问项目接口就可以实现监控了
簇点链路是单机调用链路，默认情况会监控springmvc每一个Endpoint(接口) 也就是controller层的请求
但是默认情况下簇点展示的是接口的路径，通过restful风格编设计的接口，有些路径会产生相同路径，需要开启Sentinel的请求方式前缀进行区分

```yaml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8090
      http-method-specify: true # 开启请求方式前缀
```

#### 3. 请求限流

直接点击相关路径的流控按钮就可以进行限流的的控制

<img src="https://img2.imgtp.com/2024/05/06/kN2Bp9fa.png" alt="">

#### 4. 线程隔离

线程需要隔离是因为微服务相互调用导致线程资源的占用
涉及到微服务就需要开启Feign的Sentinel功能

```yaml
feign:
  sentinel:
    enabled: true # 开启feign对sentinel的支持
```

小提示： 默认情况下tomcat最大线程数是200,允许的最大连接是8192
开启之后，远程调用微服务的请求就会被识别为一个簇点资源
然后对相关簇点进行限流就可以了

#### 5. fallback

对于触发了限流或者熔断的操作并非需要抛出异常，这是就可以使用fallback的方式，使用日志的形式来处理此类情况
给FeignClient编写失败后的降级逻辑有两种方式

1. FallbackClass 缺点：无法对远程调用的异常做出处理（偏向于快速启用的偷懒方式）
2. FallbackFactory 可以对远程调用的异常做出处理，一般采取这种方式

使用FallbackFactory的方式

1. 在公共的微服务请求转发模块建立工厂

```java
@Slf4j
public class ItemClientFallback implements FallbackFactory<ItemClient> {
    @Override
    public ItemClient create(Throwable cause) {
        return new ItemClient() {
            @Override
            public List<ItemDTO> queryItemByIds(Collection<Long> ids) {
                log.error("远程调用ItemClient#queryItemByIds方法出现异常，参数：{}", ids, cause);
                // 查询购物车允许失败，查询失败，返回空集合
                return CollUtils.emptyList();
            }

            @Override
            public void deductStock(List<OrderDetailDTO> items) {
                // 库存扣减业务需要触发事务回滚，查询失败，抛出异常
                throw new BizIllegalException(cause);
            }
        };
    }
}
```

核心部分在于继承 FallbackFactory 接口 和请求失败后的业务逻辑处理

2. 然后将工厂注册为bean

```java
    @Bean
    public ItemClientFallback itemClientFallback(){
        return new ItemClientFallback();
    }
```

3. 最后在定义的转发接口中使用工厂

```java
@FeignClient(value = "item-service",
        configuration = DefaultFeignConfig.class,
        fallback = ItemClientFallback.class)
```

#### 6. 服务熔断

熔断的过程
<img src="https://img2.imgtp.com/2024/05/07/dfekcDUQ.png">

熔断配置
<img src="https://img2.imgtp.com/2024/05/07/N76s3wgw.png">

#### 7. 持久化保存配置

使用jar包启动的控制台配置，在刷新页面就会丢失相应的配置
所有需要持久化的保存的话需要新的方法
这里使用的是nacos的方式进行配置的持久化保存

1. 引入依赖

```xml

<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
    <version>1.8.6</version>
</dependency>
```

具体的版本号要和sentinel的版本号对应

2. nacos配置文件
   在对应的nacos中配置文件，配置相应的服务熔断，请求限流和隔离的相关配置

## 分布式事务

为什么需要分布式事务：
<img src="https://img2.imgtp.com/2024/05/12/7vB1CsnP.png">

### Seata

seata角色：
<img src="https://img2.imgtp.com/2024/05/12/GWiC68Gx.png">
seara结构：
<img src="https://img2.imgtp.com/2024/05/12/yBJOBojf.png">

### TC事务管理

1.准备数据库表
TC需要对全局的事务进行管理，所以需要保存事务的状态等，就采用了持久化的保存方法

#### 微服务集成seata

seata的安装自行参考网上步骤，建议安装到虚拟机中

1. 依赖导入

```xml
<!--seata-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
</dependency>
```

2. 配置，微服务找到TC

```yaml
seata:
  registry: # TC服务注册中心的配置，微服务根据这些信息去注册中心获取tc服务地址
    type: nacos # 注册中心类型 nacos
    nacos:
      server-addr: 192.16865.128:8848 # nacos地址
      namespace: "" # namespace，默认为空
      group: DEFAULT_GROUP # 分组，默认是DEFAULT_GROUP
      application: seata-server # seata服务名称
      username: nacos
      password: nacos
  tx-service-group: hmall # 事务组名称
  service:
    vgroup-mapping: # 事务组与tc集群的映射关系
      hmall: "default"
```

配置在项目中可变性太小了，现在seata已经部署为了微服务，就可以通过nacos动态地配置
将上面的配置写入nacos的配置中
<img src="https://img2.imgtp.com/2024/05/19/OuUG8hiw.png" />
然后在微服务中创建压yaml文件 bootstrap 通过yaml配置文件读取nacos的各个配置
示例代码如下

```yaml
spring:
  application:
    name: cart-service # 服务名称
  profiles:
    active: dev
  cloud:
    nacos:
      server-addr: 192.168.65.128:8848 # nacos地址
      config:
        file-extension: yaml # 文件后缀名
        shared-configs: # 共享配置
          - dataId: shared-jdbc.yaml # 共享mybatis配置
          - dataId: shared-log.yaml # 共享日志配置
          - dataId: shared-swagger.yaml # 共享日志配置
          - dataId: shared-seata.yaml # 共享日志配置
```

再导入依赖

```xml

<dependencies>
    <!--统一配置管理-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    </dependency>
    <!--seata-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
    </dependency>
</dependencies>
```

最后重启项目就ok了

#### XA模式

XA规范：X/Open 组织定义的分布式事务处理的标准，它描述了全局的TM与局部的RM之间的接口，多数主流关系型数据库都支持XA规范
XA模式结构图：
<img src="https://img2.imgtp.com/2024/05/19/eM1BCEvj.png"/>
XA模式是两阶段的事务模式，先执行然后等待最后一起提交

1. 开启XA模式

在nacos配置文件中统一修改

```yaml
seata:
  data-source-proxy-mode: XA
```

2. 设置事务入口

在需要开启事务的方法上加上注解

```java
@GlobalTransactional
```

3. 重启项目

#### AT模式

XA模式的改进版，加入了快照，执行完后直接提交，如果有错误就回滚快照，没问题就直接删除快照
结构图：
<img src="https://img2.imgtp.com/2024/05/19/248EPtnh.png" />

缺陷：一致性弱，其中一个事务提交在回滚前会被其他事务调用，产生数据的不一致

1. 给需要回滚的数据表格添加undo_log表
<img src="https://img2.imgtp.com/2024/05/19/J8J60t38.png" />
2.  修改模式
将XA改为TA
3. 重启项目

