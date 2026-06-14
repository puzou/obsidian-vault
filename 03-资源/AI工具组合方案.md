---
tags:
  - AI工具
  - 方案
created: 2026-05-05
---

# AI 工具组合方案

> 基于使用场景、免费额度、环境适配三维度筛选，从 9 个工具精简为 4 个主力，消除功能重叠和选择纠结。

## 个人情况摘要

| 维度   | 情况                                                          |
| ---- | ----------------------------------------------------------- |
| 使用场景 | 写代码/调试、跑实验、写文档/报告/PPT、日常问答                                  |
| 开发环境 | WSL + Windows 双环境，主力编辑器 VS Code                             |
| 付费态度 | 能免则免                                                        |
| 免费资源 | Gemini API、硅基流动、火山引擎 API + Codex/Antigravity/WorkBuddy 自带额度 |
| 核心痛点 | 额度用完没替代、不知道该用哪个、工具太多没精力深入                                   |

## 推荐工具组合

### 1. Claude Code CLI — 主力代码 Agent

| 项目 | 说明 |
|------|------|
| 角色 | 代码开发第一选择 |
| 环境 | Windows CMD |
| 适用场景 | 写代码、调试、重构、读项目、复杂实验指导 |
| 额度 | 需订阅或 API key（无免费额度） |
| 保留理由 | 使用频率最高，代码能力最强，Agent 模式 + CLAUDE.md 自定义 |

### 2. GeminiCLI — 免费代码 Agent + WSL 侧

| 项目 | 说明 |
|------|------|
| 角色 | Claude Code 的免费备选 + WSL 原生环境代码工具 |
| 环境 | WSL |
| 适用场景 | WSL 下写代码/调试、Claude Code 额度用完时的降级选择 |
| 额度 | Gemini API 免费 |
| 保留理由 | 唯一原生运行在 WSL 的代码 Agent，免费额度兜底 |

### 3. WorkBuddy — 全能助手 + 国内模型入口

| 项目 | 说明 |
|------|------|
| 角色 | 非代码场景主力 + 网络受限时的保底工具 |
| 环境 | Windows 桌面端 |
| 适用场景 | 文档/报告/PPT 生成、日常问答、翻译润色、轻量代码、实验简单指导 |
| 额度 | 国内模型免费 |
| 保留理由 | 国内模型不受墙影响，文档生成+导出能力完整，免费额度充裕 |

### 4. Codex App — 免费 OpenAI 代码入口

| 项目 | 说明 |
|------|------|
| 角色 | 代码场景的第三级备份 |
| 环境 | Windows 桌面端 |
| 适用场景 | 两个 CLI 都没额度时的代码任务保底 |
| 额度 | OpenAI 免费额度 |
| 保留理由 | 独占 OpenAI 免费额度，作为最后一级降级保障 |

## 可移除的工具

| 工具 | 移除理由 | 被谁替代 |
|------|---------|---------|
| OpenCode | 与 GeminiCLI 功能重叠，无自带免费额度 | GeminiCLI |
| OpenClaw | 与 WorkBuddy 功能重叠，无独占优势 | WorkBuddy |
| Hermess | 与 WorkBuddy 功能重叠，无独占优势 | WorkBuddy |
| Antigravity | 与 Codex 功能重叠，Codex 背靠 OpenAI 模型更稳 | Codex |

> 注意：如果某个被移除的工具有你特别依赖的独占功能，可以单独保留，不影响整体组合逻辑。

## 额度备份链

解决"额度用完没替代"的核心痛点。

### 代码场景（三级降级）

```
Claude Code（最强，可能受限）
    ↓ 额度不足 / 网络问题
GeminiCLI（免费，Gemini API）
    ↓ 依然不够
Codex App（免费，OpenAI 额度，保底）
```

### 非代码场景

```
WorkBuddy（国内模型免费，不心疼额度，不受墙影响）
```

## 场景速查表

解决"不知道该用哪个"的核心痛点。

| 场景 | 首选 | 备选 | 说明 |
|------|------|------|------|
| 写代码 / 调试 / 重构 | Claude Code | GeminiCLI → Codex | CLI 三级链按顺序降级 |
| 读代码 / 理解项目 | Claude Code | GeminiCLI | 代码理解用最强的模型 |
| 跑实验（复杂） | Claude Code | — | 需要深度推理用最强模型 |
| 跑实验（简单指导） | WorkBuddy | — | 国内部署、改配置等简单任务 |
| 写文档 / 报告 / PPT | WorkBuddy | — | 生成 + 导出一条龙 |
| 日常问答 / 翻译 / 润色 | WorkBuddy | — | 国内模型免费，响应快 |
| WSL 环境下操作 | GeminiCLI | — | 唯一原生 WSL 的工具 |
| 网络受限（墙/SSL） | WorkBuddy | — | 国内模型不受影响 |
| Claude Code 额度用完 | GeminiCLI | Codex | 按备份链降级 |

## 简单决策流程

遇到任务时，按以下逻辑快速决定：

```
是否涉及写代码/调试/读项目？
├── 是 → 在 WSL 还是 Windows？
│   ├── WSL → GeminiCLI
│   └── Windows → Claude Code
│       └── 额度不够？→ GeminiCLI → Codex
└── 否 → WorkBuddy
```

## 额外 API 资源利用

| API key | 用途 | 接入方式 |
|---------|------|---------|
| 硅基流动 | 接入 DeepSeek/Qwen 等国内模型 | WorkBuddy 自定义模型 / 直接 API 调用 |
| 火山引擎 | 接入豆包等模型 | WorkBuddy 自定义模型 / 直接 API 调用 |
| Gemini API | GeminiCLI 已用 | — |

当 WorkBuddy 默认额度紧张时，可以将硅基流动或火山引擎的 key 配置到 WorkBuddy 作为补充模型源。

## 相关笔记

- [[AI工具工作空间使用规范]]
- [[AI工具Skill管理规范]]
