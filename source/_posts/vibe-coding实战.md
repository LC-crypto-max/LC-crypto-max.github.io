---

title: vibe coding实战
date: 2026-04-08 16:11:50
tags:
  - AI工程
  - Codex
  - SpringBoot
  - 后端开发
  - 工程实践

---

# 从0到1构建可控AI工程体系：AGENTS + Skill 驱动的实战

## 一、核心思路

我将 AI 开发过程抽象为三层结构：

```text
AI工程体系
├── AGENTS.md          # 规则层（约束AI行为）
├── .agents/skills     # 能力层（定义AI如何写代码）
└── .codex/config.toml # 控制层（AI运行方式）
```

### 架构说明

| 层级  | 文件          | 作用         |
| --- | ----------- | ---------- |
| 规则层 | AGENTS.md   | 定义项目规范     |
| 能力层 | Skill       | 定义AI如何生成代码 |
| 控制层 | config.toml | 控制AI运行行为   |

---

## 二、项目结构

```text
hello-code
├── src
├── pom.xml
├── AGENTS.md
├── README.md
├── docker-compose.yml
├── .agents
│   └── skills
│       └── springboot-backend-standard
│           └── SKILL.md
└── .codex
    └── config.toml
```

---

## 三、AGENTS.md（规则层）

AGENTS.md 的核心作用是：

> 约束 AI 生成代码必须符合工程规范

示例：

```md
## 项目定位
用于面试展示的 Spring Boot 后端系统

## 技术栈
- Java 17
- Spring Boot 3.3.x
- JPA + H2
- RabbitMQ
- Elasticsearch

## 代码规范
- 分层结构（controller/service/repository）
- 使用统一返回 Result<T>
- 必须有全局异常处理
- DTO / VO 分离

## 输出要求
- 必须生成 README.md
- 必须生成 demo.http
- 必须提供演示步骤
```

---

## 四、Skill 设计（能力层）

路径：

```text
.agents/skills/springboot-backend-standard/SKILL.md
```

### Skill 的本质

Skill 本质上是给 AI 定义“如何写代码的方法论”。

### 示例

```md
---
name: springboot-backend-standard
description: 生成适合面试展示的Spring Boot后端项目
---

# 技术选型
- Spring Boot 3.3.x
- JPA + H2
- RabbitMQ
- Elasticsearch

# 生成规则
1. 必须可运行
2. 必须包含 demo.http
3. 必须包含 README.md
4. 必须说明如何演示
5. 保证依赖版本兼容

# 输出内容
- Controller / Service / Repository
- 全局异常处理
- Result 统一返回
- 示例接口
```

---

## 五、Codex 配置（控制层）

路径：

```text
.codex/config.toml
```

配置内容：

```toml
project_root_markers = [".git"]

[features]
auto_search = true
auto_context = true
```

作用：

* 标记项目根目录
* 提高 AI 上下文理解能力
* 保证 AGENTS 与 Skill 能稳定生效

---

## 六、AI驱动开发流程

### 1. 指定规则与能力

```text
请使用 springboot-backend-standard skill，
并遵循 AGENTS.md，
生成检测结果管理接口
```

---

### 2. AI自动生成内容

AI会自动生成：

* Controller
* Service
* Repository
* demo.http
* README
* 接口示例

---

### 3. 本地运行

```bash
mvn spring-boot:run
```

---

### 4. 数据库控制台访问

```text
http://localhost:8080/h2-console
```

---

### 5. 数据验证

```sql
SELECT * FROM detection_result;
```

---

## 七、项目演示效果

项目运行效果如下：

![IDEA运行界面](/images/IDEA运行项目.png)

数据库查询结果如下：

![H2查询结果](/images/数据库查询.png)

示例数据：

```json
{
  "defectType": "crack",
  "defectLevel": "high",
  "summary": "检测到明显裂缝，长度约12cm"
}
```

---

## 八、核心收获

本项目的核心不在业务，而在工程方法。

### 收获

* 构建了 AGENTS 约束 AI 行为
* 构建了 Skill 控制代码生成方式
* 形成可复用的 AI 开发模板
* 实现 AI 生成代码的工程化落地

### 避免的问题

* AI 随意生成代码
* 项目结构混乱
* 依赖版本冲突
* 项目无法运行或演示

---

## 九、总结

通过 AGENTS + Skill + Codex 配置，实现了：

> 从“使用AI写代码”到“让AI参与工程开发”的转变

---
