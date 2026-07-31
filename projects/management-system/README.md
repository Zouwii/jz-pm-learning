# Management-System

> 2026-07-31 事实修正：旧 RAG 文档、旧 26 题测试集和旧实验仅作为 legacy baseline。当前已获得新的约 55G 图文知识库（5,427 份 Markdown、19,886 张图片），需要围绕 Markdown AST、表格、图片资产、OCR/视觉 artifact、父子 chunk、增量重建和新评测集重新设计 RAG v2。下文旧链路与指标不得直接当作新系统结果。

## 一句话

面向本体开发团队的 2B 数据与 AI 工作台，把钉钉知识库、TB 任务和组织数据连接起来，提供知识问答、任务辅助和管理分析。

## 这个项目证明什么

这是 AI PM 主项目。旧版本证明我做过完整 RAG 和评测闭环；RAG v2 要进一步证明我能发现旧方案的数据基础错误，并基于真实大规模图文数据重新定义问题、架构和验收标准：

- 从内部真实痛点中定义产品问题；
- 判断工程方案、RAG 和 Agent 的能力边界；
- 搭建数据同步、分块、混合检索、对话和 MCP 全链路；
- 建立测试集与离线指标，用实验而不是感觉做优化决策；
- 同时考虑权限、部署、性能和用户工作流。

## 项目讲述主线

### 背景与问题

团队知识散落在多个钉钉知识库和 TB 任务中。员工检索成本高，任务描述不规范；管理者难以快速获得结构化信息。钉钉原生能力存在跨空间、检索黑盒和业务数据融合限制。

### 方案

```text
钉钉知识库 / TB 数据
        ↓
同步、解析、四类自适应分块
        ↓
FULLTEXT/FTS + bge-small 向量检索
        ↓
RRF 融合 + Cross-Encoder reranker
        ↓
带来源引用的 LLM 对话
        ↓
Skills / MCP 执行业务动作
```

### Legacy 评测闭环

- 用三路检索 Pooling 构建 26 条测试问题；
- 形成 135 个相关 chunk 标注，测试集无空题；
- 使用 Recall@5、MRR、nDCG@5 衡量检索质量；
- 对比 Hybrid RRF baseline 与 Cross-Encoder reranker；
- legacy 数据上的可核验结果：Recall@5 从 34.13% 提升到 59.60%，约提升 25.5 个百分点；
- 最终选择 `bge-reranker-base`，原因是服务器 CPU 约 1 秒/query，效果与成本更平衡。

### 我的产品判断

1. 先优化检索质量，再讨论 Prompt，因为检索错误是上游根因。
2. 先用小而可解释的测试集建立 baseline，再逐步扩大在线反馈。
3. 不盲目换大 embedding；reranker 在当前数据上收益更明确。
4. 平台提供基础能力，自建部分聚焦内部数据、权限和工作流差异。

## 三种讲法

### 30 秒

我先完成过一版钉钉知识库 RAG 和离线评测，但在获得约 55G 原始 Markdown 与图片后，确认旧解析、旧文档和旧测试集不能代表真实数据。现在我把它升级为 RAG v2：重新设计 AST/表格/图片资产、OCR 与原图关联、父子 chunk、幂等重建和包含图片问题的新评测集。旧结果只保留为 baseline，新的产品价值必须由新数据上的对照实验重新证明。

### 2 分钟

按“用户问题 → legacy 方案与评测 → 为什么旧数据基础不成立 → 55G 新数据审计 → RAG v2 设计 → canary 与新评测 → 迁移边界”展开。

### 5 分钟

在 2 分钟版基础上加入：

- Markdown AST、HTML 清洗与表格行组；
- 图片 asset、OCR、视觉摘要和原图回链；
- 父子 chunk、Hybrid RRF 与 reranker；
- checksum 幂等重建、ACL 和灰度迁移；
- 新 40 题评测集及图片关联指标；
- 尚未完成的 canary、在线指标和用户采纳闭环。

## 风险与口径

- legacy 26 题规模较小，只能证明旧链路中的局部方向；
- legacy 标注使用 LLM 辅助，需要人工复核；
- `34.13%→59.60%` 只能作为旧数据 baseline，不能宣传为 RAG v2 结果；
- 新 55G 数据尚未完成 canary 和新评测，RAG v2 当前状态必须描述为“重构中”；
- 不把离线 Recall、OCR 准确率或图片关联准确率直接等同于用户满意度。

## 证据

- [需求来源](./01-需求来源.md)
- [AI PM 全流程复盘](./02-AI-PM全流程复盘.md)
- RAG v2 设计：`management-system/docs/ai/rag/RAG-v2-markdown-multimodal-design.md`
- 实验工作台：`management-system/backend/ai/knowledge/rag_improve/`
- 源仓库：`/home/zhr/zhr_ws/src/ai/management-system`
