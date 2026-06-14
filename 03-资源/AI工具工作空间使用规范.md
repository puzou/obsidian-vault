---
tags:
  - AI工具
  - 规范
created: 2026-05-05
---

# AI 工具工作空间使用规范

## 核心原则

**以项目为工作空间，不要以 AI 工具为工作空间。**

项目是核心，AI 工具是工具。工具跟着项目走，不是项目跟着工具走。

## 错误做法

按 AI 工具创建工作空间，所有项目混在一起：

```
C:\Develop\Python\
├── ClaudeCodeWorkSpace\      ← 所有项目混在一起
│   ├── feature-matching 代码
│   ├── 小脚本
│   ├── 笔记草稿
│   └── .claude\
├── GeminiCLIWorkSpace\       ← 同样混乱
└── CodexWorkSpace\           ← 同样混乱
```

问题：代码、笔记、配置全堆一块，找不到东西，工具之间无法协作。

## 正确做法

按项目创建目录，多个 AI 工具共享同一个项目目录：

```
D:\Projects\
├── feature-matching-benchmark\     ← 项目目录
│   ├── .claude\                    ← Claude Code 配置（自动生成）
│   ├── .codebuddy\                 ← WorkBuddy 配置（自动生成）
│   ├── .workbuddy\                 ← WorkBuddy 记忆（自动生成）
│   ├── CLAUDE.md                   ← Claude Code 项目指令
│   ├── LoFTR\
│   ├── TopicFM\
│   └── hloc\
├── another-project\
│   ├── .claude\
│   ├── .codebuddy\
│   └── ...
└── Obsidian Vault\                 ← 笔记项目
    ├── .codebuddy\
    └── ...
```

## 隐藏目录互不影响

同一个项目下多个 AI 工具的配置目录不会互相干扰：

| 隐藏目录          | 归属           | 说明                |
| ------------- | ------------ | ----------------- |
| `.claude/`    | Claude Code  | 只被 Claude Code 读取 |
| `.codebuddy/` | WorkBuddy    | 只被 WorkBuddy 读取   |
| `.workbuddy/` | WorkBuddy 记忆 | 只被 WorkBuddy 读取   |
| `venv/`       | Python 虚拟环境  | 只被对应环境读取          |
| `.git/`       | Git          | 只被 Git 读取         |

这些目录：
- 都是隐藏目录（以 `.` 开头）
- 各自的工具只读自己的目录
- 其他工具会自动忽略不认识的隐藏目录
- Git 默认也会忽略它们（或加到 `.gitignore`）

类比：VS Code 生成 `.vscode/`，PyCharm 生成 `.idea/`，打开同一个项目互不干扰。AI 工具同理。

## WSL 特殊说明

WSL 下的 GeminiCLI 通过 `/mnt/d/Projects/xxx` 访问 Windows 文件系统，对 Windows 上的其他工具没有影响。如果 GeminiCLI 在 WSL 侧有配置目录，Windows 侧根本看不到。

## 迁移步骤

1. 按项目重新组织目录结构
2. 不用搬 `.claude`——在项目目录下重新启动 Claude Code 会自动生成
3. 不用搬 `.codebuddy`——WorkBuddy 新建工作空间指向项目路径即可
4. `venv` 跟着项目走——每个项目有自己的 conda/venv 环境
5. 旧的 `ClaudeCodeWorkSpace` 等目录清空后可以删除
6. 在项目根目录的 `.gitignore` 中添加：
   ```
   .claude/
   .codebuddy/
   .workbuddy/
   venv/
   .idea/
   .vscode/
   ```

## 相关笔记

- [[AI工具组合方案]]
- [[AI工具Skill管理规范]]
