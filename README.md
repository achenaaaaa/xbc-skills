# xbc-skills — AI Agent Skill 作品集

基于 **WorkBuddy** 平台构建的 4 个 AI Agent Skill，覆盖**经济学研究、求职投递、商业分析、公文撰写**四大场景。每个 Skill 都是一套可复用的「提示词 + 工作流 + 模板」系统，让 AI 从通用助手变成垂直领域的专业执行者。

## Skill 总览

| Skill | 定位 | 核心能力 |
|---|---|---|
| **econ-lit-review** | 经济学文献综述 | 缺口分析 → 分层检索 → PDF 核验 → 结构化综述 → 引用格式统一，全流程自动化 |
| **qiuzhao-analysis** | 秋招岗位分析与适配 | 公司深度背调（行业/财务/舆情多平台）、JD-简历匹配评分、投递策略与笔面试备考 |
| **jobhuntbot** | 求职投递工作流系统 | 档案 → 看板 → 筛选规则 → 简历策略 → 投递执行 → 卡点处理，配套本地可视化看板 |
| **gongwen-biganzi** | 公文撰写四合一 | 万事转公文（SVG 卡片）、政策文件三段式解读、深度研究报告（源治理+置信度标注）、政府标书辅助撰写 |

## 仓库结构

```
xbc-skills/
├── econ-lit-review/        经济学文献综述工作流
│   ├── SKILL.md
│   └── references/         文献追踪模板、PDF 提取指南
├── qiuzhao-analysis/       秋招岗位分析与适配
│   ├── SKILL.md
│   └── references/         多平台口碑搜索指南、简历模板（脱敏）
├── jobhuntbot/             求职投递工作流 + 本地看板
│   ├── SKILL.md
│   ├── references/         初始化流程、浏览器操作手册、安全边界
│   ├── templates/          候选人档案/规则/简历策略/题库模板
│   └── dashboard/          零依赖本地看板（HTML + Node.js）
└── gongwen-biganzi/        公文撰写四合一
    ├── SKILL.md
    ├── prompt.lisp         原始 Lisp 伪代码 Prompt
    └── *.md                政策解读 / 深度研究 / 标书撰写模板
```

## 设计特点

- **可复用的工作流**：每个 Skill 都是「触发条件 → 前置步骤 → 分步流程 → 输出规范 → 边界约束」的完整系统，而非一次性提示词。
- **真实可追溯**：研究报告强调源治理与置信度标注；文献综述要求「仅引用已下载并核验的文献」；投递系统要求「只记录已确认的投递」——贯穿反幻觉、反编造原则。
- **安全边界内自治**：求职投递 Skill 明确「绝不猜测身份/薪资等高风险信息」「最终提交前必须用户确认」，体现对自动化风险的克制。
- **中文场景适配**：针对中国顶刊引用范式、央企校招流程、党政公文规范做了本地化适配。

## 版权与致谢

- `jobhuntbot` 基于 [DanielPan12/JobHuntBot](https://github.com/DanielPan12/JobHuntBot)（MIT 许可）改编，已适配 WorkBuddy 与中国秋招场景。
- `gongwen-biganzi` 核心「万事转公文」源于李继刚（出处 [lykayang.github.io#101](https://github.com/lykayang/lykayang.github.io/issues/101)），政策解读 / 深度研究 / 标书撰写为后续扩展。
- 其余 Skill 为原创构建。

## 隐私说明

仓库中的简历、看板数据均为**脱敏示例或空模板**，不含任何真实个人信息。
