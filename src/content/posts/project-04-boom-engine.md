---
title: "项目 4 · 爆点引擎 Boom-Engine"
published: 2026-10-05
description: "基于 FastAPI + Redis + SQLAlchemy 的高并发秒杀系统：Lua 原子扣库存、用户限购、IP 滑动窗口限流，附压测脚本，杜绝超卖。"
tags: ["FastAPI", "Redis", "高并发", "Lua", "项目"]
category: 项目
draft: false
---

## 项目简介

基于 **FastAPI + Redis + SQLAlchemy** 构建的高并发秒杀实战系统，支持原子库存扣减、用户限购、IP 限流，附带压测脚本，杜绝超卖。

## 核心特性

- **原子库存扣减** —— Lua 脚本在 Redis 内原子执行，杜绝超卖
- **用户限购** —— 同一用户只能抢购一次（Redis Set 判重）
- **IP 限流** —— 滑动窗口计数器，每个 IP 可配置最大 QPS
- **库存预热** —— 启动时自动将库存加载到 Redis
- **秒杀重置** —— 一键恢复库存、清空订单
- **数据库可切换** —— 一行环境变量从 SQLite 切换到 PostgreSQL
- **连接池复用** —— Redis 连接池 + SQLAlchemy 连接池，支撑高并发
- **异步全链路** —— FastAPI 全异步 + `redis.asyncio` + `httpx` 压测

## 设计要点

**为什么扣库存一定要放进 Lua。** 秒杀场景下最典型的写法是"先查库存，再扣减，再落单"——三个操作之间只要有窗口，高并发下就会超卖。把判断与扣减合并成一段在 Redis 内部原子执行的 Lua 脚本，"检查 + 扣减"之间就不存在可插入的时间窗口。

**限流放在扣库存之前。** 顺序很关键：先用 IP 滑动窗口把明显异常的流量挡在外面，再判重（同一用户只能抢一次），最后才动库存。这样真正落到库存上的请求量已经被削过两轮。

**连接池 + 全异步。** 高并发下瓶颈往往不在业务逻辑而在连接建立。Redis 连接池和 SQLAlchemy 连接池复用连接，配合 `redis.asyncio` 与 FastAPI 的异步链路，避免 IO 等待把 worker 拖死。

## 项目结构

```
高并发秒杀网页/
├── main.py              # FastAPI 主应用 & 秒杀接口
├── config.py            # 配置（Redis、库存、限流阈值）
├── models.py            # SQLAlchemy ORM 模型 & 数据库操作
├── redis_client.py      # Redis 异步连接池管理
├── lua_scripts.py       # Redis Lua 脚本（扣库存 & 限流）
├── test_concurrency.py  # 高并发压测脚本
├── static/
│   └── index.html       # 前端秒杀页面
└── redis/               # Windows Redis 可执行文件
```

## 技术栈

| 层 | 技术 |
|---|---|
| Web 框架 | FastAPI（异步） |
| 缓存 & 原子操作 | Redis + Lua 脚本 |
| 数据库 | SQLAlchemy ORM（SQLite / PostgreSQL） |
| 前端 | 原生 HTML/CSS/JS（暗色渐变 UI） |
| 压测 | httpx + asyncio 并发测试脚本 |

::github{repo="rensunan/Boom-Engine"}
