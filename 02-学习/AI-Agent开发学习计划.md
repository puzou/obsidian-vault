---
tags:
  - 学习
  - AI-Agent
  - LLM
date: 2026-05-07
---

# AI Agent 开发学习计划

> Agent = LLM + Tools + Memory + Planning，核心循环：Think → Act → Observe → Repeat

## Phase 1：基础储备（2-3 周）

### 1. Python 异步编程
- `asyncio` / `await` 基本语法
- `aiohttp` 异步 HTTP 请求——Agent 大量并发调用 API，异步是必须的
- 不需要深入到事件循环原理，会用就行

**学习资源**：
- 📺 [尚硅谷 2025 Python 教程（禹神主讲）](https://www.bilibili.com/video/BV1yG4y1J77s) — B站免费，含异步开发章节，讲解节奏友好
- 📺 [Python异步编程——asyncio小白速通](https://www.bilibili.com/video/BV1KmUpB8EJ3/) — B站18集系列，从协程到实战
- 📖 [Python异步编程完全教程：asyncio/aiohttp核心用法与实战](https://www.cnblogs.com/xyash/p/19328129) — 博客，系统讲解异步全流程
- 📖 [廖雪峰 Python 教程 - asyncio](https://liaoxuefeng.com/books/python/async-io/asyncio/) — 经典入门，简明扼要
- 📖 [菜鸟教程 Python asyncio](https://www.runoob.com/python3/python-asyncio.html) — 速查手册

### 2. LLM API 调用实战
- 用 OpenAI API（或国内兼容接口如 SiliconFlow/Volcengine）完成对话
- 理解 token、temperature、system/user/assistant 角色
- **动手项目**：写一个命令行聊天机器人，支持多轮对话 + 流式输出

**学习资源**：
- 📺 [黑马程序员大模型 RAG 与 Agent 智能体项目实战](https://www.bilibili.com/video/BV1yjz5BLEoY) — B站，基于 LangChain，52小时覆盖 Prompt→RAG→Agent 全流程
- 📺 [2026最新 AI Agent 开发教程](https://www.bilibili.com/video/BV1K7RsBnEaK/) — B站，从入门到实战全覆盖
- 📖 [AI 智能体实战速成指南](https://didilili.github.io/ai-agents-from-zero/) — 开源电子书，系统教程+可跑源码+面试题库
- 📖 [黑马大模型 RAG 与 Agent 实战学习笔记](https://blog.csdn.net/qq_54693844/article/details/159348073) — CSDN，配套代码实验 30+ 个
- 📖 OpenAI 官方 API 文档 — 最权威的 API 参考手册

> 核心产出：能流畅调 LLM API，理解对话机制，不害怕异步代码

---

## Phase 2：核心能力（3-4 周）

### 1. Function Calling（工具调用）
- LLM 不只是聊天，它能决定"调用哪个函数、传什么参数"
- 理解 JSON Schema 描述工具 → LLM 返回调用意图 → 代码执行 → 结果回传
- **动手**：让 LLM 调用天气 API、计算器、文件读写

**学习资源**：
- 📖 [从零构建大模型智能体：OpenAI Function Calling 实战](https://blog.csdn.net/yjw123456/article/details/155879095) — CSDN，从原理到代码全流程
- 📖 [详解 OpenAI 函数调用 Function Calling](https://www.cnblogs.com/damoxing/articles/19011124) — 博客，清晰讲解工作原理
- 📖 [OpenAI Function Calling 完整指南](https://hrefgo.com/blog/openai-function-calling-complete-guide) — 含 Python/JS 示例与故障排除
- 📖 [OpenAI开发系列：Function calling 流程优化](https://zhuanlan.zhihu.com/p/645732735) — 知乎，深入推理流程
- 🎓 吴恩达 [Functions, Tools and Agents with LangChain](https://www.deeplearning.ai/short-courses/functions-tools-agents-langchain/) — DeepLearning.AI 免费课

### 2. RAG（检索增强生成）
- 文本切分 → Embedding → 向量数据库（ChromaDB / FAISS）→ 检索 → 注入 prompt
- 这是 Agent 的"知识库"，没有 RAG 的 Agent 只能用训练数据里的知识
- **动手**：给一个 PDF 文档做问答（用 ChromaDB + OpenAI Embedding）

**学习资源**：
- 📖 [All-in-RAG | RAG 技术全栈指南](https://datawhalechina.github.io/all-in-rag/) — Datawhale 开源项目，体系化学习路径+实践
- 📄 [飞书文档：RAG 实践](https://docs.feishu.cn/article/wiki/LV0PwkBqNioeR4kK8QIcSkSJnCN) — 飞书，从简介到四步构建RAG
- 📖 [从原理到落地：RAG 技术全解析](https://juejin.cn/post/7561104926819090466) — 掘金，手把手搭建知识库
- 📺 搜索 B站 "RAG 教程" — 大量实操演示视频

### 3. Memory（记忆系统）
- 短期记忆：对话上下文窗口
- 长期记忆：向量存储 + 摘要
- 工作记忆：当前任务的 scratchpad（Agent 的"草稿纸"）
- **动手**：给聊天机器人加长期记忆，能记住之前说过的事

**学习资源**：
- 📖 [Lilian Weng: LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) — Memory 章节讲得最透彻（英文原版）
- 📄 [飞书翻译版：LLM 驱动的自主 Agents](https://docs.feishu.cn/article/wiki/WPnWwi8z8i9LqKkOIaCc9VpqnRe) — 中文翻译，方便阅读
- 📖 [Lilian Weng 博客学习笔记](https://blog.csdn.net/weixin_45838549/article/details/136748893) — CSDN 中文解读

### 4. Prompt Engineering 进阶
- 不只是写好 prompt，而是设计 Agent 的"人格"和"行为规范"
- Few-shot / CoT（Chain-of-Thought）/ ReAct 模式
- **动手**：用 ReAct 模式实现一个能推理+行动的 Agent

**学习资源**：
- 📖 [ChatGPT Prompt工程：CoT与ReAct策略深度解析](https://developer.baidu.com/article/detail.html?id=4808890) — 百度开发者，原理+实现+场景
- 📖 [Prompt中的CoT和ReAct](https://zhuanlan.zhihu.com/p/659102403) — 知乎，含原始论文解读
- 📖 [Prompt工程完全指南](https://blog.csdn.net/gitblog_00531/article/details/155155136) — CSDN，中文用户专版
- 🎓 吴恩达 [ChatGPT Prompt Engineering for Developers](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) — DeepLearning.AI 免费

> 核心产出：能从零写一个 ReAct Agent，有工具、有记忆、有知识库

---

## Phase 3：框架实战（3-4 周）

### 框架选择（选一个深入即可）

| 框架 | 特点 | 适合谁 |
|------|------|--------|
| **LangGraph** | 图结构编排，灵活可控，LangChain 生态 | 想精细控制流程的人 |
| **CrewAI** | 多角色协作，上手快 | 想快速搭 Multi-Agent 的人 |
| **AutoGen** | 微软出品，对话式多 Agent | 偏研究/实验场景 |

**建议先学 LangGraph**：
1. 市场需求最大，LangChain 生态最全
2. 图结构思维能帮你理解所有 Agent 编排的本质
3. 学会了 LangGraph，其他框架看文档一周就能上手

**框架学习资源**：

***LangGraph***：
- 📺 [黑马程序员大模型 RAG 与 Agent 智能体项目实战](https://www.bilibili.com/video/BV1yjz5BLEoY) — B站，LangChain 生态一条龙（含 LangGraph）
- 📺 [尚硅谷 LangChain 教程](https://www.bilibili.com/video/BV1ZppNzHEY4) — B站，LangChain 从入门到实战
- 📺 [2026吴恩达 LangGraph 保姆级教程](https://www.bilibili.com/video/BV1kCPmzAEXb/) — B站中英双字，附课件代码
- 📺 [2025大模型 LangGraph 保姆级教程](https://www.bilibili.com/video/BV11LL8zUE8b) — B站，从底层原理到项目实战
- 📖 [LangGraph 从入门到精通](https://blog.csdn.net/qq_41797451/article/details/145562267) — CSDN，手把手构建 AI 智能体
- 📖 [LangGraph 中文文档](https://langchain-doc.cn/v1/python/langgraph/) — 官方文档中文翻译
- 📖 [LangGraph 官方 Tutorials（中文镜像）](https://github.langchain.ac.cn/langgraph/tutorials/) — 最权威教程
- 🎓 吴恩达 [AI Agents in LangGraph](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/) — DeepLearning.AI 免费
- 📦 [Datawhale Agentic AI](https://github.com/datawhalechina/agentic-ai) — 吴恩达 Agent 课程中文版，结构化整理+实践引导

***CrewAI***：
- 📖 [CrewAI 多 Agent 协作实战](https://www.cnblogs.com/czlws/p/19858139/crewai-multi-agent-collaboration-tutorial) — 博客，完整工作流构建
- 📖 [CrewAI 上手攻略](https://cloud.tencent.com/developer/article/2593966) — 腾讯云，角色分工+流程编排
- 📖 [LLM之Agent：多智能体框架 CrewAI](https://zhuanlan.zhihu.com/p/681218725) — 知乎，框架原理与对比

***AutoGen***：
- 📖 [多智能体协同深度指南](https://jimmysong.io/zh/book/ai-handbook/agent/multi-agent/) — Jimmy Song，LangGraph/AutoGen/CrewAI 全覆盖
- 📖 [2025多Agent协作网络｜CrewAI & AutoGen](https://www.explongs.com/blog/yt-2025-multi-agent-crewai-autogen/) — 分布式任务编排对比

### 动手项目（挑 2 个做）
- [ ] 用 LangGraph 做一个"研究助手"：给定主题 → 搜索 → 总结 → 生成报告
- [ ] 用 CrewAI 做一个"代码评审团队"：Reviewer + Tester + Writer 三个角色协作
- [ ] 用 LangGraph + RAG 做一个"文档问答 Agent"：支持多轮追问 + 来源引用

---

## Phase 4：进阶方向（持续）

- **Multi-Agent 编排**：多个 Agent 协作完成复杂任务，MCP 协议
- **评估体系**：Agent 输出质量怎么衡量？Latency / Accuracy / Cost / 成功率
- **部署上线**：FastAPI 包装 → Docker → 云部署 → 监控日志
- **垂直领域**：结合专业方向——比如深度学习实验管理的 Agent

**学习资源**：

***MCP 协议***：
- 📖 [AI Agent 开发新范式 MCP 从入门到全链路实战](https://cloud.tencent.com/developer/article/2561899) — 腾讯云，核心概念+搭建流程
- 📖 [2026年AI Agent开发实战：MCP协议深度解析](https://juejin.cn/post/7628131346834063410) — 掘金，MCP+多智能体实战
- 📖 [MCP协议实战：让AI Agent互联互通](https://jishuzhan.net/article/2048947042639216641) — 2026新基建视角

***Multi-Agent***：
- 📖 [多智能体协同深度指南](https://jimmysong.io/zh/book/ai-handbook/agent/multi-agent/) — Jimmy Song AI 手册，系统性讲解
- 📖 [Lilian Weng: LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) — Agent 理论天花板

***评估与部署***：
- 📖 LangSmith 官方文档 — LangChain 生态的 Agent 评估/追踪工具
- 📖 [Building LLM Apps](https://www.oreilly.com/library/view/building-llm-apps/) — 实用导向，覆盖部署

---

## 通用学习资源

| 类型 | 资源 | 说明 |
|------|------|------|
| 📖 博客 | [Lilian Weng: LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) | Agent 理论天花板，必读 |
| 📄 飞书 | [LLM 驱动的自主 Agents（中文翻译）](https://docs.feishu.cn/article/wiki/WPnWwi8z8i9LqKkOIaCc9VpqnRe) | Lilian Weng 博客中文版 |
| 📖 开源书 | [AI 智能体实战速成指南](https://didilili.github.io/ai-agents-from-zero/) | 系统教程+可跑源码+面试题库 |
| 📦 GitHub | [Datawhale Agentic AI](https://github.com/datawhalechina/agentic-ai) | 吴恩达 Agent 课程中文版 |
| 🎓 课程 | [DeepLearning.AI 短课合集](https://www.deeplearning.ai/short-courses/) | 吴恩达系列，全部免费 |
| 📺 B站 | 搜索 "AI Agent 开发" | 大量中文实操视频 |
| 💻 实践 | GitHub OpenHands / Claude Code | 直接看开源 Agent 怎么写的 |

### 培训机构课程汇总

> 黑马和尚硅谷的 B站免费版内容已够用，无需报付费班。结合自身深度学习基础，跟课速度会很快。

| 机构 | 课程 | 覆盖阶段 | 说明 |
|------|------|---------|------|
| 🏫 黑马程序员 | [大模型 RAG 与 Agent 智能体项目实战](https://www.bilibili.com/video/BV1yjz5BLEoY) | Phase 2-3 | 52小时，LangChain 一条龙，配套[学习笔记](https://blog.csdn.net/qq_54693844/article/details/159348073) |
| 🏫 黑马程序员 | [AI大模型开发学习路线图](https://yun.itheima.com/subject/aimap/index.html) | 全阶段 | 官方路线图，了解行业视角 |
| 🏫 尚硅谷 | [2025 Python 教程（禹神主讲）](https://www.bilibili.com/video/BV1yG4y1J77s) | Phase 1 | 含异步开发章节，讲解节奏友好 |
| 🏫 尚硅谷 | [LangChain 教程](https://www.bilibili.com/video/BV1ZppNzHEY4) | Phase 3 | LangChain 从入门到实战 |
| 🏫 尚硅谷 | [AI 大模型技术系列课](https://www.51xt.com/course/61497ae5a4) | Phase 2-3 | NLP + LangChain + Agent 全覆盖 |

---

## Agent 核心架构

```
        ┌─────────────────────┐
        │     LLM (Brain)     │
        │  推理 + 决策 + 规划  │
        └──┬──────────────┬───┘
           │              │
     ┌─────▼─────┐  ┌────▼──────┐
     │   Tools    │  │  Memory   │
     │ 搜索/代码  │  │ 短期/长期 │
     │   /API     │  │ /工作记忆  │
     └─────┬─────┘  └────┬──────┘
           │              │
     ┌─────▼─────┐  ┌────▼──────┐
     │    RAG     │  │ Planning  │
     │ 知识检索   │  │ 任务拆解  │
     │ + 向量库   │  │ + 执行策略 │
     └───────────┘  └───────────┘
           │              │
           └──────┬───────┘
         ┌────────▼────────┐
         │     Observe     │
         │ 观察结果 + 反馈  │
         └────────┬────────┘
                  │ (loop back to LLM)
```

> 关键建议：别只看教程不动手。每个阶段至少做一个小项目。做出来 > 完美主义。
