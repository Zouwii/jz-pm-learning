# JZ Product 原始材料

本目录保留原始产品分析、PRD 和系统设计。它属于证据层，不是日常 interview 的第一入口。

| 文件 | 用途 | 对应项目 |
| --- | --- | --- |
| [全向车 CE 方案阅读](./03-PRD阅读学习-全向车CE方案.md) | PRD 阅读与业务理解练习 | Nav-Application |
| [AGV 云端健康运维平台 PRD](./04-AGV云端健康运维平台-PRD.md) | 工业运维产品设计参考 | Diagnosis-Agent |
| [Diagnosis-Agent PRD](./05-技术支持诊断Agent-PRD.md) | 原始需求、用户、范围和路线图 | Diagnosis-Agent |
| [运维退化趋势分析](./06-运维退化趋势分析-需求分解.md) | 从故障诊断向趋势运维扩展的需求拆解 | Diagnosis-Agent |
| [Diagnosis-Agent 系统架构](./07-技术支持诊断Agent-系统架构设计.md) | LangGraph、多 Agent、证据与降级设计 | Diagnosis-Agent |

使用规则：

1. 日常讲述先看 `projects/*/README.md`。
2. 被追问“为什么这样设计”时再查本目录。
3. 本目录保持原始产物；统一的 interview 口径写回 `projects/`。
4. 原稿与实际代码冲突时，以源仓库和可运行状态为准。
