---
title: RabbitMQ入门
date: 2026-03-18
tags:
  - RabbitMQ
  - 消息队列
  - Java
  - 中间件
categories:
  - 中间件
cover: 
---

# RabbitMQ入门

> 💡 本文基于实际学习，适合快速掌握RabbitMQ核心原理

---

## 🧠 一、什么是RabbitMQ

RabbitMQ 是一个基于 **AMQP协议** 的消息中间件，用于实现系统之间的通信。

✨ 本质作用：
- 解耦系统
- 异步处理
- 流量削峰

---

## 🎯 二、为什么要使用RabbitMQ？

### 🚀 1. 解耦
系统之间不直接调用，提高扩展性  

### ⚡ 2. 异步
提高接口响应速度  

### 📈 3. 削峰
应对高并发流量  

---

## 🏗 三、核心架构

> 💡 RabbitMQ最核心的一张图


---

## 🖼️ 架构图解析

{% asset_img hello-world-example-routing.webp RabbitMQ示意图 %}

---

## 🔑 四、核心组件

### 🔹 Producer（生产者）
发送消息的一方  

### 🔹 Exchange（交换机）🔥
负责路由消息（核心）

### 🔹 Queue（队列）
存储消息  

### 🔹 Consumer（消费者）
处理消息  

---

## 🔥 五、四种Exchange（重点）

---

### 1️⃣ Direct（直连模式）

```text
routingKey = error
```

👉 精确匹配

---

### 2️⃣ Fanout（广播模式）

👉 无视routingKey

✔️ 发送给所有队列

👉 适用于：

* 日志广播
* 系统通知

---

### 3️⃣ Topic（通配符模式🔥）

```text
*  匹配一个单词
#  匹配多个单词
```

例：

```text
order.*
order.#
```

👉 最常用交换机类型

---

### 4️⃣ Headers（了解即可）

👉 根据header匹配

---

## 🔄 六、RabbitMQ工作模式

---

### 🟢 简单模式

```
Producer → Queue → Consumer
```

---

### 🟡 工作队列模式

👉 多消费者分摊任务

👉 特点：

* 一条消息只会被一个消费者处理

---

### 🔵 发布订阅模式

👉 使用 Fanout

👉 特点：

* 广播消息

---

### 🔴 路由模式

👉 使用 Direct

👉 特点：

* 精准分发

---

## ⚙️ 七、安装RabbitMQ（Docker）

```bash
docker run -d \
--name rabbitmq \
-p 5672:5672 \
-p 15672:15672 \
rabbitmq:3-management
```

访问：

```
http://localhost:15672
```

账号密码：

```
guest / guest
admin / 123456
```

---

## 💻 八、Java示例（生产者）

```java
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");

Connection connection = factory.newConnection();
Channel channel = connection.createChannel();

channel.queueDeclare("hello", false, false, false, null);

String message = "Hello RabbitMQ!";
channel.basicPublish("", "hello", null, message.getBytes());

System.out.println("发送成功");

channel.close();
connection.close();
```

---

## 💻 九、消费者代码

```java
channel.basicConsume("hello", true, (consumerTag, message) -> {
    System.out.println("收到消息：" + new String(message.getBody()));
}, consumerTag -> {});
```

---

## 🚀 十、高级特性（面试重点🔥）

### ✅ 消息确认机制（ACK）

防止消息丢失

---

### ✅ 死信队列（DLQ）

处理异常消息

---

### ✅ 延迟队列

用于：

* 订单超时
* 定时任务

---

### ✅ 持久化

保证消息不丢

---

## 🖼️ RabbitMQ管理界面

{% asset_img 安装rabbitMQ服务器.png RabbitMQ示意图 %}
{% asset_img rabbitMQ-UI.png RabbitMQ示意图 %}

接收信息示例：
{% asset_img 接受信息示例.png RabbitMQ示意图 %}

---

## 🎯 十一、总结

> 💡 RabbitMQ核心 = Exchange路由机制

✔️ 掌握这三点即可理解80%：

* Exchange
* RoutingKey
* Queue

---

# 🚀 END
