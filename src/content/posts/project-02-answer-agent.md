---
title: "项目 2 · 解疑答惑 Agent"
published: 2026-10-07
description: "基于 DeepSeek + LangChain + FastAPI + Vue 的智能答疑助手：RAG 知识库问答、联网搜索、文档解读、多轮对话与用户体系。"
tags: ["RAG", "LangChain", "FastAPI", "Vue", "项目"]
category: 项目
draft: false
---

## 项目简介

基于 DeepSeek + LangChain + FastAPI + Vue.js 构建的智能客服答疑助手。

## 功能

- 📚 **知识库问答（RAG）** —— Chroma 向量数据库 + bge-large-zh 检索 + bge-reranker-v2-m3 重排序
- 🌐 **联网搜索** —— DuckDuckGo 实时搜索互联网
- 📄 **文档解读** —— 上传 PDF / DOCX / TXT / MD 文件智能分析
- 🔍 **问题诊断** —— 逐步引导用户排查技术问题
- 💬 **多轮对话** —— 摘要压缩记忆，超过 20 轮自动 AI 压缩历史
- 👤 **用户体系** —— 账号密码登录注册，对话历史持久化
- 🎨 **现代 UI** —— Vue.js 侧边栏布局，暗色模式，消息操作
- 📊 **数据管理** —— 对话搜索、重命名、导出 Markdown、统计面板
- 🔄 **流式响应** —— SSE 实时流式输出

## 设计要点

**检索链路用了两段式。** 单独用向量召回（bge-large-zh）在长文档场景下经常把“看起来像”的片段排到前面，所以召回之后再加一层 bge-reranker-v2-m3 做重排序，把真正相关的段落顶上来——精度提升比单纯换更大的 embedding 模型更划算。

**上下文控制靠摘要而不是截断。** 超过 20 轮就自动压缩历史，避免直接丢弃早期信息导致“聊着聊着忘了前面说过什么”。

**前端零构建。** Vue 3 走 CDN 单文件，不需要 Node 工具链，把部署复杂度压到最低。

## 技术栈

| 组件 | 技术 |
|------|------|
| LLM | DeepSeek API（OpenAI 兼容） |
| Agent 框架 | LangChain |
| 后端 | FastAPI |
| 前端 | Vue 3（CDN 单文件） |
| 向量数据库 | Chroma |
| Embedding | bge-large-zh-v1.5 |
| 重排序 | bge-reranker-v2-m3 |
| 数据库 | SQLite |
| 样式 | 纯 CSS + 暗色模式 |

::github{repo="rensunan/answer-agent"}
