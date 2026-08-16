# JobHuntBot

一个由 AI Agent 驱动的求职投递工作流，配一个本地求职进度追踪看板。适用于 WorkBuddy 及其他能读取文件、执行文字指令的 AI Agent。

基于 [DanielPan12/JobHuntBot](https://github.com/DanielPan12/JobHuntBot)（MIT 许可），改编自 Yvonne He 的 ApplyPilot 工作流。已针对 WorkBuddy + 中国秋招场景做适配。

## 仓库结构

```
SKILL.md                        Agent 核心工作流与安全约定 —— 从这里开始
references/
  setup-workflow.md             分步初始化流程
  application-playbook.md       浏览器/ATS 操作手册
  safety-and-boundaries.md      隐私、知情同意、安全边界
templates/
  candidate_profile.template.json
  application_rules.template.md
  resume_routing.template.md
  answer_bank.template.md
  experience_bank.template.md
  dashboard-template/           空白 CSV 看板 + 字段说明
dashboard/                      本地进度看板
  server.js                     零依赖静态文件服务器
  dashboard.html                看板界面
  start-dashboard.bat / .sh     一键启动脚本
  *.csv                         空白数据文件
```

## 快速开始

1. 告诉 Agent：「使用 jobhuntbot skill 帮我初始化求职工作流」
2. Agent 会引导填写候选人档案、规则、简历策略
3. 先做一次「仅找岗位」试运行
4. 打开看板查看进度

## 安全边界

- 不猜测身份、工作资格、薪资
- 不绕过验证码/反爬机制
- 不编造经历或学历
- 最终提交前一定等用户确认

## 许可证

MIT
