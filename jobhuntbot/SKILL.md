---
name: jobhuntbot
description: "秋招求职投递工作流与进度追踪系统。适用场景：用户需要初始化求职系统（候选人档案、看板、筛选规则、简历策略）、搜索和筛选岗位、在安全边界内执行投递、记录投递结果、处理卡点、或迭代求职策略。也适用于将已有 qiuzhao-analysis 的公司分析结果接入结构化投递流程。"
agent_created: true
---

# JobHuntBot — 秋招求职投递工作流

JobHuntBot 是一个 AI Agent 驱动的求职投递操作系统。帮助用户将求职过程变成一套可重复的系统：档案 → 看板 → 筛选规则 → 简历策略 → 投递执行 → 卡点处理 → 跟进。

> 基于 DanielPan12/JobHuntBot（MIT 许可），改编自 Yvonne He 的 ApplyPilot 工作流。已针对 WorkBuddy + 中国秋招场景做适配。

## 核心约定

优化目标：**真实、可追溯、能产生面试机会的投递**，而非盲目追求数量。

将初始化视为 Agent 引导的入职流程，而非用户的作业清单。只询问安全启动所需的最少信息，为用户创建草稿/模板，然后在首次试运行后迭代。

在搜索或投递之前，确保用户已具备：

1. 候选人档案（candidate_profile）
2. 进度看板（dashboard CSV + HTML）
3. 投递筛选规则（application_rules）
4. 简历策略（resume_routing）
5. 明确的安全边界（浏览器操作 + 表单答案）

若缺少任何一项，先初始化。**绝不猜测**身份、法律、工作资格、薪资、当前就业、落户/调动等高风险信息。

## 与 qiuzhao-analysis 的分工

若用户已安装 `qiuzhao-analysis` skill：

| 步骤 | 使用工具 |
|:---|:---|
| 公司基本面分析、行业前景 | qiuzhao-analysis |
| JD-简历匹配度分析 | qiuzhao-analysis |
| 社交平台评价/面试经验 | qiuzhao-analysis |
| 投递进度追踪、卡点管理 | **jobhuntbot（本 skill）** |
| 看板可视化、跟进日程 | **jobhuntbot（本 skill）** |
| 结构化投递流程执行 | **jobhuntbot（本 skill）** |

两者互补——qiuzhao-analysis 做「分析研判」，jobhuntbot 做「执行追踪」。

## 工作流

### 1. 初始化系统

首次使用或用户要求创建档案/看板/规则/模板时，读取 `references/setup-workflow.md`。

使用 `templates/` 中的模板创建用户文件：

- `candidate_profile.json` ← `templates/candidate_profile.template.json`
- `application_rules.md` ← `templates/application_rules.template.md`
- `resume_routing.md` ← `templates/resume_routing.template.md`
- `answer_bank.md` ← `templates/answer_bank.template.md`
- `experience_bank.md` ← `templates/experience_bank.template.md`
- `dashboard/*.csv` ← `templates/dashboard-template/*.csv`

针对中国秋招场景特别注意：
- 目标公司优先国企/央企（如中国人保省级分公司）
- 投递渠道：央企官网校招页面、国聘网（iguopin.com）、应届生求职网
- 校招批次以「2027届」为准，注意区分秋招/春招/提前批

### 2. 确认公司并调研岗位

当用户指定目标公司时，逐家处理：

- 检查 `job_pool.csv` 是否已有该公司记录，若无则新增
- 确认目标届别（2027届）的校招是否真正开放——搜索公司官方校招页面的具体申请入口，交叉验证发布时间
- 警惕「实习已确认、全职未确认」和「届别标签与实际资格窗口不符」的陷阱
- 将调研结果立即写入 `job_pool`（job_url, next_action, notes, status）
- 每次完成调研后显式设置 `cohort_match_status`（Yes/No/Unclear）

### 3. 投递前筛选

按以下优先级排序：时效性 > 匹配度 > 可行性 > 转化概率。

- 默认优先处理过去 24 小时内的岗位，其次 48 小时
- 跳过或延迟：违反用户规则的、明显超 level 的、已关闭或重复的、需要不支持的工作资格的、缺少必要材料的、长表单且匹配度低的

### 4. 精确岗位短列表供用户选择

确认公司开放后，不要直接填表。先找到匹配用户目标岗位族的具体职位，呈现短列表：

- 岗位名称
- 一句话匹配度说明
- 级别/资格要求
- 工作地点
- 是否为 2027 届秋招全职（非实习、非往届）

若公司限制每人只能投 1-2 个岗位，在用户选择前说明。

### 5. 匹配经历到岗位

在接触申请之前，决定为此岗位展示哪些经历：

- 参考 `experience_bank.md` 中目标岗位族的候选池（每个岗位族建议 3-5 段实习 + 3+ 个项目，评级 强/中）
- 阅读 JD，从候选池中选择 2-4 段最匹配的经历
- **使用前告知用户选择了哪些经历及原因**（清单 + 一句话理由）
- 在填写简历相关自由文本字段时使用选中的经历（非完整清单）
- 投递后在 `application_log` 的 notes 中记录实际使用的经历
- 如实原则：只能重排序、选择、强调；绝不虚构、夸大或拉伸经历

### 6. 简历策略路由

使用用户选择的策略：

- **精准模式**：筛选高匹配度岗位 → 定制简历后再投递
- **海投模式**：使用预制的岗位族简历变体，快速推进

默认使用海投模式。可将特定高匹配岗位从海提升至精准。

### 7. 填写申请表

操作浏览器前读取 `references/application-playbook.md`。

> ⚠️ 中国秋招注意：央企校招通常通过官网投递系统（而非 Greenhouse/Workday），表单结构差异大。优先梳理目标公司的实际投递流程，而非套用英文 ATS 手册。

优先上传简历让系统自动解析（比手动输入教育/经历更少出错）。从 `candidate_profile.json`、`resume_routing.md`、`experience_bank.md`、`answer_bank.md` 中提取能自信填写的内容。遇到 never_guess 清单中的项目、需要主观判断的项目、或表单中出现简历/档案之外的内容时，停下来询问用户。

遇到验证码、Cloudflare、反爬、登录/2FA、法律/身份不明确的问题、缺少文件、付款提示、权限提示时，停止或转交用户。

### 8. 预览 → 用户确认 → 提交

最终点击提交前，向用户展示摘要（公司、岗位、简历版本、选中的经历、关键回答、薪资信息）。

**在用户明确说「提交」之前，绝不点击最终提交按钮。** 预览页面不等于同意。

提交后，看到真实的确认证据（成功文字、感谢页 URL、候选人 ID）后再记录为 `Submitted`。

### 9. 同步看板和档案

每个岗位线索或投递尝试必须以以下状态之一结束：

- `Submitted`：确认证据已观察到
- `Skipped`：不值得投递，附原因
- `Blocked`：自动化无法完成，附卡点和下一步
- `Needs user`：用户需提供缺失的高风险信息或完成 CAPTCHA/登录/上传
- `Pending`：选择稍后处理，等待前提条件满足

只计入已确认的投递。已收藏、追踪中、快速投递标签都不算。

**仅找岗位的试运行**：更新 `job_pool`、`daily_dashboard`、`blocker_queue`、`automation_rules`；不写 `application_log`（因为没有投递尝试）。

真正投递时：更新所有看板文件，并将表单填写过程中发现的新事实写回 `candidate_profile.json`。

记录投递时，将 JD 完整文本（职责和要求）按原文复制到 `application_log` 的 `job_description` 字段——这使后续面试准备不需要重新寻找可能已下线的岗位页面。

### 10. 从卡点中学习

每次运行后总结卡点，将重复问题转化为规则。JobHuntBot 应通过使用持续改进。

## 安全

当用户询问自动化边界、验证码、邮箱验证、账户登录、隐私、公开分享或不应包含在仓库中的内容时，读取 `references/safety-and-boundaries.md`。

不要将私人简历、电话号码、邮箱、地址、身份证明文件、投递历史、浏览器会话、Cookie、验证码或用户特定机密发布到公开的 JobHuntBot 包中。

## 启动看板

看板是纯静态 HTML + 零依赖 Node.js 本地服务器：

- Windows：双击 `dashboard/start-dashboard.bat`
- macOS/Linux：运行 `dashboard/start-dashboard.sh`（需要 Node.js）
- 浏览器打开 `http://localhost:8420/dashboard.html`

通过启动脚本打开，不要直接双击 HTML 文件——页面需要通过 HTTP 读取 CSV 数据。
