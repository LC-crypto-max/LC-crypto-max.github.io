---
title: Redis 学习笔记
date: 2026-03-16 17:12:49
categories:
  - 中间件
tags:
  - Redis
  - 数据库
---

# Redis 学习笔记

## 一、Redis简介

Redis（Remote Dictionary Server）是一个高性能的键值型数据库。

## 二、Redis架构示意图

![Redis架构图](/images/redis-architecture.jpg)

图：Redis整体架构
---

## 三、Redis核心数据结构

Redis支持以下几种主要数据结构：

| 数据结构 | 说明 |
|------|------|
| String | 字符串 |
| List | 列表 |
| Set | 集合 |
| Hash | 哈希 |
| Zset | 有序集合 |

示例：

```bash
set name lyc
get name