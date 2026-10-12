# RSN · 个人站点

基于 [Astro](https://astro.build) 构建的个人博客与简历站点，用来记录学习笔记、项目实践与技术思考。

线上地址：**https://jackport.app.workbuddy.host/**

## 站点内容

- **个人简历** —— 教育背景、工作经历、项目经验、技能特长
- **项目实践** —— 6 个项目的实现记录，涵盖 AI Agent、RAG 检索、地理信息分析与云存储：
  - Companion-Agent · 陪伴智能体
  - answer-agent · 知识问答智能体
  - GeoWise-Analyzer · 岩土工程表格智能解析
  - Boom-Engine · 内容创作引擎
  - Smart-Cloud-Storage · 智慧云存储
  - Agent-create-skill · 智能体技能生成

## 技术栈

| 类别 | 选型 |
| --- | --- |
| 框架 | Astro 5（静态站点生成） |
| 交互 | Svelte 5 岛屿组件 |
| 样式 | Tailwind CSS 3 + Stylus |
| 搜索 | Pagefind（纯静态全文检索） |
| 内容 | Markdown（Astro Content Collections） |
| 代码高亮 | Expressive Code |
| 数学公式 | KaTeX + remark-math / rehype-katex |

## 本地开发

前置依赖：Node.js >= 20、pnpm >= 9。

```bash
pnpm install --ignore-scripts   # Windows 下 esbuild postinstall 会报 EBUSY，加此参数绕过
pnpm dev                        # 开发服务：http://localhost:4321
```

构建与预览：

```bash
pnpm build      # astro build + pagefind 生成搜索索引
pnpm preview    # 本地预览构建产物
```

## 新增文章

在 `src/content/posts/` 下新建 `.md` 文件，frontmatter 格式：

```yaml
---
title: 文章标题
published: 2026-10-10
description: 文章摘要
tags: [标签一, 标签二]
category: 分类
draft: false
---
```

首页与归档页的排序由 `published` 字段控制。

## 目录结构

```
src/
├── config.ts           # 站点配置：标题、导航栏、侧栏资料、主题色
├── content/
│   ├── posts/          # 博客文章（Markdown）
│   └── spec/about.md   # 关于页内容
├── components/         # UI 组件
├── layouts/            # 页面布局
├── i18n/               # 多语言文案
├── plugins/            # 自定义 remark / rehype 插件
└── styles/             # 全局样式与设计变量
public/favicon/         # 站点图标
astro.config.mjs        # Astro 配置：集成、Markdown 插件、Vite
```

站点身份集中在 `src/config.ts`，改站点名、导航、侧栏资料、主题色都在这一个文件里。

## 部署

站点是纯静态产物，构建后部署 `dist/` 目录即可，无需服务端。
