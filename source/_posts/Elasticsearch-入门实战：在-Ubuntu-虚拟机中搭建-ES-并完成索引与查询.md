---
title: Elasticsearch 入门实战：在 Ubuntu 虚拟机中搭建 ES 并完成索引与查询
date: 2026-03-25
tags:
  - Elasticsearch
  - Linux
  - 搜索引擎
categories:
  - 中间件
---

## 1. 为什么学习 Elasticsearch

Elasticsearch（简称 ES）是一个基于 Lucene 的分布式搜索与分析引擎，常用于全文检索、日志分析、商品搜索和数据查询等场景。  
对于初学者来说，最重要的不是先背大量概念，而是先把服务跑起来，再通过“创建索引、写入文档、执行查询”建立整体认识。

---

## 2. 实验环境

- Ubuntu 22.04 虚拟机
- Elasticsearch 9.x
- curl 命令行工具

---

## 3. 安装 Elasticsearch

### 3.1 导入官方 GPG key

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
````

### 3.2 添加仓库

```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
```

### 3.3 安装 ES

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https
sudo apt-get install -y elasticsearch
```

---

## 4. 启动 Elasticsearch

```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
```

如果状态显示 `active (running)`，说明 ES 已成功启动。

---

## 5. 设置 elastic 用户密码

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

然后把密码保存为环境变量：

```bash
export ELASTIC_PASSWORD='你的密码'
```

---

## 6. 验证服务是否正常

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200
```

如果返回包含 `cluster_name`、`version` 等字段的 JSON，说明服务已运行成功。

---

## 7. 创建索引

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X PUT "https://localhost:9200/student" \
-H "Content-Type: application/json" \
-d '
{
  "mappings": {
    "properties": {
      "name":  { "type": "text" },
      "age":   { "type": "integer" },
      "major": { "type": "keyword" }
    }
  }
}'
```

这个索引中定义了 3 个字段：

* `name`：文本类型
* `age`：整数类型
* `major`：关键字类型

---

### 索引创建过程示意

{% asset_img curl实战.png 索引创建示意图 %}

---

## 8. 插入文档

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X POST "https://localhost:9200/student/_doc/1" \
-H "Content-Type: application/json" \
-d '
{
  "name": "Alice",
  "age": 22,
  "major": "computer"
}'
```

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X POST "https://localhost:9200/student/_doc/2" \
-H "Content-Type: application/json" \
-d '
{
  "name": "Bob",
  "age": 24,
  "major": "mechanical"
}'
```

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X POST "https://localhost:9200/student/_doc/3" \
-H "Content-Type: application/json" \
-d '
{
  "name": "Cindy",
  "age": 22,
  "major": "computer"
}'
```

### 查询结果示意

{% asset_img 查询演示.png 查询示意图 %}

---

## 9. 查询示例

### 9.1 查询全部数据

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X GET "https://localhost:9200/student/_search" \
-H "Content-Type: application/json" \
-d '
{
  "query": {
    "match_all": {}
  }
}'
```

### 9.2 查询专业为 computer 的学生

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X GET "https://localhost:9200/student/_search" \
-H "Content-Type: application/json" \
-d '
{
  "query": {
    "term": {
      "major": "computer"
    }
  }
}'
```

### 9.3 查询年龄为 22 的学生

```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD \
-X GET "https://localhost:9200/student/_search" \
-H "Content-Type: application/json" \
-d '
{
  "query": {
    "term": {
      "age": 22
    }
  }
}'
```

---

## 10. 总结

通过这次实验，我完成了 Elasticsearch 单机环境的搭建，并实践了以下核心操作：

1. 安装并启动 ES 服务
2. 使用 HTTPS 和账号密码访问 ES
3. 创建索引和字段映射
4. 插入文档数据
5. 进行基础查询

