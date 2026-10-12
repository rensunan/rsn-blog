---
title: "项目 5 · 智慧云存 Smart-Cloud-Storage"
published: 2026-10-04
description: "基于 Cloudflare Workers + KV 的个人云存储网页应用：多分类管理、拖拽上传下载、加密便利贴、存储统计与实时搜索。"
tags: ["Cloudflare Workers", "Serverless", "JavaScript", "项目"]
category: 项目
draft: false
---

## 项目简介

一个个人云存储网页应用，支持多分类管理、文件上传下载、加密便利贴，基于 **Cloudflare Workers + KV** 构建。

## 功能

| 功能 | 说明 |
|------|------|
| 📂 **分类管理** | 图片 / 小说 / 文档 / 便利贴，可自由扩展 |
| ➕ **CRUD 操作** | 新建、查看详情、编辑、删除 |
| 📎 **文件上传** | 支持拖拽上传，最大 10MB，图片自动预览 |
| ⬇️ **一键下载** | 文件原格式下载，文本导出 `.txt` |
| 🔒 **便利贴** | 账号密码存储，密码自动遮蔽，点击 👁️ 查看 |
| 📊 **存储统计** | 彩色进度条展示各类别存储占用 |
| 🔍 **实时搜索** | 搜索标题、内容、文件名 |
| 📱 **响应式** | 手机端侧边栏折叠，深色主题 |

## 设计要点

**零服务器的取舍。** 选 Cloudflare Workers + KV 是为了把"部署"这件事彻底消掉——没有服务器要维护，没有数据库要备份，免费额度对这种个人规模绰绰有余。代价是 KV 最终一致且没有复杂查询能力，所以数据结构刻意做得很扁平：按分类分 key，不做关联查询。

**扩展分类只改两个文件各一行。** 分类体系是配置驱动的，新增一个分类不需要动业务逻辑，这是刻意的设计——个人项目最怕的就是"想加个功能得先读懂半年前写的代码"。

**前端零框架。** 原生 HTML/CSS/JS，构建脚本只做资源内联。整个应用体积很小，首屏加载快，也没有框架版本升级的维护负担。

## 技术栈

- **后端**：Cloudflare Workers（ES Modules）
- **存储**：Cloudflare KV（本地开发模拟）
- **前端**：原生 HTML/CSS/JS，零框架依赖
- **构建**：Node.js 构建脚本内联资源
- **部署**：Cloudflare Workers / Netlify

## 本地运行

```bash
# 1. 安装 Wrangler
npm install -g wrangler

# 2. 进入项目
cd 个人云数据存储

# 3. 启动开发服务器
wrangler dev

# 4. 打开浏览器
# http://localhost:8787
```

::github{repo="rensunan/Smart-Cloud-Storage"}
