# Application Playbook

Use this reference for browser-based job applications and common ATS pages.

## Global Rules

- Count only confirmed submissions.
- Prefer short, reliable paths over long custom forms.
- Close each completed or skipped job tab before moving on.
- Keep only tabs that need user handoff.
- Record every outcome in the dashboard.
- Stop rather than bypass verification or guess high-impact answers.
- Do not create separate "test" and "normal" behavior modes. Use one default behavior: automate clear low-risk fields, ask focused questions for missing high-impact facts, and always stop before final submit.

## Form Answer Defaults

- Basic fields with clear profile values can be filled automatically: name, email, phone, LinkedIn, location, resume upload, and start date.
- Work authorization, sponsorship, and compensation can be filled only when wording matches the profile or answer bank closely.
- Voluntary self-ID defaults to blank, "Prefer not to say", or decline/skip when available unless the user configured exact answers.
- Custom questions should use answer-bank patterns when available. If no pattern exists, draft the specific answer and ask the user to confirm it.
- Final submit always requires user approval. Show a concise summary before the final click.

## Low-Friction Applications

Volume mode and first real application tests should prefer low-friction applications:

- No new account creation.
- Not Workday, Oracle, or another long enterprise ATS by default.
- No video, long writing sample, or mandatory portfolio submission.
- At most one custom question.
- Clear resume upload and final confirmation path.
- No CAPTCHA, Cloudflare, login, or 2FA interruption.

This is a prioritization rule, not a permanent ban. In Precision mode, a high-value role may justify longer forms after the user confirms it is worth the extra time.

## Chinese Campus Recruitment Notes (中国秋招)

State-owned enterprise (央企/国企) applications differ significantly from English ATS:

- **Custom portals**: Most SOEs use their own application systems, not standard SaaS ATS. Each portal has unique form flows.
- **Common fields**: 政治面貌 (政治面貌), 生源地, 学籍验证报告, 外语水平 (CET-4/6), 计算机等级
- **Resume upload**: Many portals support .doc/.docx/.pdf upload with auto-parse for key fields
- **Account requirements**: Often need 手机号 + 身份证号 for registration
- **Session timeout**: SOE portals frequently have short session timeouts — save progress often
- **Application limits**: Many SOEs limit to 1-2 position submissions per person in the same cycle
- **Review cycles**: SOEs typically have longer review cycles with multiple stages (简历筛选 → 笔试 → 面试 → 体检 → 政审)

### Common Chinese Platforms

| Platform | Type | Notes |
|:---|:---|:---|
| 国聘网 (iguopin.com) | SOE job board | Aggregates many SOE postings; has its own application flow |
| 应届生求职网 | Campus board | Good for discovering openings; often redirects to company portal |
| 各央企官网校招页面 | Company portal | The actual application destination; varies by company |
