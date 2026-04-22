# Vault42 — LLM-Driven Personal Knowledge Wiki

> A template for building personal knowledge bases powered by LLMs, inspired by Karpathy's Wiki methodology.

---

## 核心思想

传统 RAG（检索增强生成）的工作方式：上传文档 → 查询时检索片段 → 生成回答。LLM 每次回答都要重新发现知识，没有积累。

**本系统的核心区别**：LLM **增量构建和维护一个持久的 Wiki**。

当你添加一篇新资料时，LLM 不是简单地索引它供后续检索，而是：
- 阅读原始资料
- 提取关键信息
- 将知识整合到现有的 Wiki 中
- 更新相关实体页、概念页
- 标注新旧信息的矛盾
- 强化或挑战已有的综合分析

Wiki 是一个**持久的、不断累积的工件**。交叉引用已经建立，矛盾已经被标注，综合分析已经反映了你的所有阅读。

> 你永远不需要自己维护这些交叉引用——LLM 会做所有的工作。

---

## 灵感来源

本系统基于 Karpathy 的 [LLM Wiki](https://github.com/karpathy/llm-wiki) 理念：

> *"The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."*

Karpathy 的愿景是让 LLM 成为 Wiki 的维护者，人类负责：
- 策展来源
- 探索分析
- 提出好问题
- 思考意义

LLM 负责：
- 总结、交叉引用、归档、维护

---

## 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                     三层架构                                 │
├─────────────────────────────────────────────────────────────┤
│  Raw（原始资料层）                                            │
│  ├── 你的文章、论文、图片、数据文件                              │
│  ├── 不可变，LLM 只读取不修改                                   │
│  └── 来源：用户的策展                                          │
├─────────────────────────────────────────────────────────────┤
│  Wiki（知识库层）                                             │
│  ├── AI 生成和维护的 Markdown 文件                            │
│  ├── 实体页、概念页、来源摘要页、对比分析页                       │
│  └── AI 写入，人类阅读                                         │
├─────────────────────────────────────────────────────────────┤
│  Schema（SCHEMA.md）                                          │
│  ├── 告诉 LLM 如何组织 Wiki 的配置                             │
│  ├── 约定、格式、工作流                                         │
│  └── 人类和 AI 共同演进                                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 快速开始

### 1. 初始化

克隆本模板后，首次打开会看到：

```
\
├── SCHEMA.md         ← AI 的行为手册（必读）
├── README.md         ← 本文件
├── Raw/              ← 放你的原始资料
└── Wiki/              ← AI 会在这里构建知识库
```

> **提示**：Methodology.md 是方法论参考，可从 [Karpathy/llm-wiki](https://github.com/karpathy/llm-wiki) 获取。

### 2. 添加资料

将 Markdown 文件放入 `Raw/` 目录，然后告诉你的 AI Agent：

> "请摄取这篇文章"

AI 会：
1. 阅读文件内容
2. 与你讨论关键要点
3. 创建来源摘要页
4. 更新相关的实体/概念页
5. 更新索引
6. 追加日志

### 3. 提问

直接在对话中提问：

> "我之前读过哪些关于 X 的资料？"
> "能把 A 和 B 两种方法做个对比吗？"

AI 会：
1. 搜索 Wiki 中的相关页面
2. 综合已有知识生成回答
3. 如果回答有价值，建议归档为新页面

### 4. 健康检查

定期运行：

> "检查一下 Wiki 的健康状况"

AI 会检查：
- 矛盾的说法
- 孤儿页面（没有入链）
- 提到但缺页的概念
- 过时的信息

---

## 目录结构说明

| 目录 | 用途 | 谁可以修改 |
|------|------|-----------|
| `Raw/` | 原始资料存档 | **只有用户**放入文件，AI 归档 |
| `Wiki/` | 知识库页面 | **只有 AI** |
| `SCHEMA.md` | Schema 配置 | 用户和 AI 共同演进 |

### Wiki 页面类型

| 类型 | 说明 |
|------|------|
| `来源页` | 每篇资料的摘要，如 `Wiki/来源/2026-04-22 标题.md` |
| `概念页` | 原理、方法、术语，如 `Wiki/概念/检索增强生成.md` |
| `实体页` | 人、公司、产品，如 `Wiki/实体/OpenAI.md` |
| `对比页` | 跨来源横向分析，如 `Wiki/对比/RAG vs 记忆.md` |

---

## 工具链

- **Obsidian** — Wiki 的 IDE，支持图谱视图、本地搜索、插件生态
- **LLM Agent** — 任何支持 Schema 的 Agent（通过 SCHEMA.md 理解规范）
- **Git** — 版本控制天然的协作和历史功能

### 推荐 Obsidian 插件

| 插件 | 用途 |
|------|------|
| [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) | 查询页面 frontmatter |
| [Obsidian Git](https://github.com/denolehov/obsidian-git) | 自动备份到 Git |
| [Web Clipper](https://obsidian.md/clipper) | 将网页转为 Markdown |
| [Marp](https://github.com/obsidianmd/obsidian-marp) | 从 Markdown 生成幻灯片 |

---

## 工作流示例

### 个人研究

```
1. 用 Web Clipper 收藏一篇论文到 Raw/
2. 告诉 AI："摄取这篇论文"
3. AI 创建来源摘要页，提取关键概念
4. AI 更新相关概念页（如已有）或创建新概念页
5. 一周后问："这个月我读了哪些关于注意力机制的论文？"
6. AI 综合回答，并建议归档到对比页
```

### 书籍阅读

```
1. 把每章笔记存入 Raw/
2. AI 积累后，自动生成角色、主题、情节的实体页
3. 读完后，你有一个完整的伴侣 Wiki
4. 类似 Tolkien Gateway（粉丝维基），但 AI 做所有维护工作
```

---

## 为什么选择这个系统？

### 维护成本 ≈ 零

传统 Wiki 的问题是：维护负担随内容增长而加速。人类会因为忘记更新交叉引用、忘记同步矛盾而放弃。

LLM 不会无聊，不会忘记更新交叉引用，可以一次修改 15 个文件。**维护成本接近于零。**

### 知识累积

每次回答问题、每次摄取资料，都在为 Wiki 增加价值。不是在消费知识，而是在**建造知识**。

### 即时可用的答案

好的答案可以直接归档为 Wiki 页面。你的探索、你的分析、你的发现——这些有价值的内容不会消失在对话历史中。

---

## 参考资源

- [Karpathy LLM Wiki](https://github.com/karpathy/llm-wiki) — 原始理念
- [Obsidian](https://obsidian.md/) — 本系统的 IDE
- [Memex (1945)](https://en.wikipedia.org/wiki/Memex) — Vannevar Bush 的愿景，比互联网更接近这个概念

---

## 开始使用

1. Fork 或克隆本模板
2. 用 Obsidian 打开目录
3. 读 `SCHEMA.md` 了解 Schema 规范
4. 把第一篇资料放入 `Raw/`
5. 告诉你的 AI Agent："请摄取"

---

*本模板适合作为 GitHub 知识库模板分发。工具中立，任何支持 Schema 的 LLM Agent 均可使用。*