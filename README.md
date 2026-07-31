# AI PM interview 准备库

## 唯一目标

所有材料只服务于一件事：用三个真实项目证明自己的 AI PM 能力。

| 项目 | 核心证明 | interview 定位 |
| --- | --- | --- |
| [Nav-Application](./projects/nav-application/README.md) | 工业机器人业务、复杂系统、跨模块落地 | 真实行业与工程基本盘 |
| [Management-System](./projects/management-system/README.md) | 2B 产品从需求到 RAG 评测优化 | AI PM 主项目 |
| [Diagnosis-Agent](./projects/diagnosis-agent/README.md) | Agent 工作流、能力边界、证据闭环 | AI Agent 主项目 |

三个项目组成一条递进叙事：

```text
Nav-Application
真实工业现场与复杂问题
        ↓
Management-System
把分散知识和业务数据变成可检索、可评测的 AI 产品
        ↓
Diagnosis-Agent
让 AI 从“回答问题”升级为“采集证据、分析问题、输出结论”
```

## 使用顺序

1. 2026-08-14 前先执行 [15 天冲刺表](./interview/15天冲刺表.md)。
2. 再读三个项目目录下的 `README.md`，练熟每个项目的 30 秒、2 分钟和 5 分钟版本。
3. 用 [三项目题库](./interview/三项目题库.md) 练真实项目问题。
4. 用 [当前状态](./interview/当前状态.md) 保存每次训练结论，让下一次 Codex 直接续上。
5. 被追问产品设计细节时，再进入 [jz-product 原始产品材料](./jz-product/README.md)。
6. 回答必须区分：个人主责、团队背景、已完成事实、规划设计。
7. 没有数据支撑的结果不编数字，统一标记为待补证据。

## 三层分档怎么用

| 层 | 目录 | 用法 |
| --- | --- | --- |
| 讲述层 | `projects/` | 每次 interview 前必读；存放已经收敛的项目故事和统一口径 |
| 执行/训练层 | `interview/` | 用冲刺表推进开发，用题库练回答，用当前状态跨 Codex 会话续练 |
| 证据层 | `jz-product/` + 三个源仓库 | 被追问 PRD、架构、需求边界和实现状态时查证，不要求全文背诵 |

## 一键进入助手模式

推荐在新 Codex 会话输入：

```text
$ai-pm-interview
```

也可以直接说：

```text
进入 interview 模式
```

助手会读取 `interview/当前状态.md`，恢复三个项目口径并直接开始最高优先级问题。

## 仓库边界

除保留的 `jz-product/` 原始产品材料外，本仓库不再维护：

- 泛 AI 知识百科和固定日课；
- PNC、硬件 PM、机器人 PM 等旧方向；
- company 地图、玄学时间窗口和过期 apply 计划；
- 与三个项目无关的新 PRD 练习。

需要补 LLM、RAG、Agent 知识时，直接回到对应项目问题中学习。
