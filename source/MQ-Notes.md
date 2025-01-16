---
title: "RabbitMQ笔记"
date: 2024-05-19 22:38:00
tags: [ "java","mq" ]
---

## workQueue

任务队列，一个队列绑定多个消费者，没配置前队列中的消息被消费者平均分配

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        prefetch: 1 # 每次获取1条消息，处理完了才能获取下一条
```

通过上面的配置，就使得普通队列会通过消费者的处理快慢来分配任务

## FanoutExchange

消息传递到交换机后，会发送给每一个队列

### 使用步骤

1. 构建交换机
2. 构建队列
3. 绑定交换机和队列
4. 在消费者中监听队列

## DirectExchange

消息传递给交换机后，根据规则指定发送给固定的queue，被称为定向路由
步骤和FanoutExchange 一致，只是构建队列时需要指定对应的routingkey,发送消息时也要指定对应key。
一个队列可以有多个key

## TopicExchange

key的设置有通配符的参与。
key与通配符使用 "." 分割开，#代表0或者多个单词，*代表一个单词

## 基于注解开发

```java
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "direct"),
            exchange = @Exchange(name = "hmall.direct",type = ExchangeTypes.DIRECT),
            key = {"red","blue"}
    ))
    public void listenDirectQueue(String msg) {
        System.out.println("消费者2接收到了消息:"+msg+ LocalTime.now());
    }
```

## 消息转换器

依赖的引入

```xml
        <!--Jackson-->
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

装配bean

```java
    @Bean
    public MessageConverter jacksonMessageConvertor(){
        return new Jackson2JsonMessageConverter();
    }
```