# Claude Skills Pro — 15 Battle-Tested Engineering Skills for Claude Code

[![skills.sh](https://skills.sh/b/Hahaknight/claude-skills-pro)](https://skills.sh/Hahaknight/claude-skills-pro)

**让你的 Claude Code 从"聊天助手"变成"资深工程搭档"。**
Turn Claude Code from a chat assistant into a staff-level engineering partner.

**👉 先看 [真实输出示例 EXAMPLES.md](EXAMPLES.md)——描述会说谎，输出不会。** 📖 中文手册[免费试读：Skills 机制 + 15 行场景速查表](https://hahaknight.github.io/claude-skills-pro/manual-sample-zh.html) · [EN handbook sample](https://hahaknight.github.io/claude-skills-pro/manual-sample-en.html)。想自己写 skill？读[《如何编写高质量的 Claude Code Skill》](https://hahaknight.github.io/claude-skills-pro/how-to-write-skills.html)。

每个 skill 都是一套完整的专家工作流：不是提示词片段，而是"什么时候触发 + 按什么步骤做 + 什么算违规"的工程规范，直接固化成 Claude 的行为。
Each skill encodes a complete expert workflow — trigger conditions, step-by-step procedure, and hard anti-patterns — baked into Claude's behavior.

---

## 免费样品 Free Samples（本仓库，MIT）

| Skill | 干什么 | When |
|---|---|---|
| **pr-reviewer** | 七维度系统化审查（正确性/安全/性能/契约/错误处理/测试/可维护性），只报真实问题并分级 | review 改动、提 PR 前 |
| **test-forge** | 生成能抓住真 bug 的测试：边界、失败路径、变异自检 | 补测试 |
| **bug-hunter** | 根因定位：复现→二分→单一假设→最小修复→回归测试，禁止 shotgun 修补 | 调试、修 bug |
| **feature-spec** | 写码前一页纸方案：范围、接口契约、边界情况、测试场景 | 做新功能 |
| **commit-craft** | 拆逻辑提交、防密钥泄漏、写讲清 why 的提交信息 | 提交代码 |
| **changelog-release** | 按契约变化算 semver + 用户视角 Release Notes | 发版、写 Changelog |
| **ai-code-reviewer** | AI 生成代码八类缺陷核对（幻觉 API、行为漂移、过度工程） | review AI 写的代码 |

```bash
# 生态标准安装（推荐——works with Claude Code, Cursor, Codex, Gemini CLI, OpenCode and 75+ agents）
npx skills add Hahaknight/claude-skills-pro

# 装进其他 agent：--agent codex / gemini-cli / qwen-code / goose / crush / cursor …
npx skills add Hahaknight/claude-skills-pro --agent gemini-cli

# 或手动安装（macOS/Linux/Git Bash）
git clone https://github.com/Hahaknight/claude-skills-pro.git
cd claude-skills-pro/free-skills
cp -r */ ~/.claude/skills/

# 免下载试用单个 skill（打印生成的提示词）
npx skills use Hahaknight/claude-skills-pro@pr-reviewer
```

Windows PowerShell: 把 `free-skills/*` 复制到 `C:\Users\<你>\.claude\skills\` 即可。

---

## Pro 完整包（8 个付费 + 7 个免费 = 15 个）/ Full Pro Pack

| Skill | 一句话 |
|---|---|
| security-audit | 真实攻击面安全审查（不是背 OWASP），分级 + 具体修复 |
| refactor-surgeon | 行为保持式重构：先特征测试、小步提交、等价验证 |
| perf-profiler | 先测量后优化：profile→热点→修→复测，每个结论带数字 |
| codebase-onboarding | 快速建立项目心智模型 + "加 X 改哪里"速查表 |
| api-designer | 不会破坏兼容的 API 设计：错误契约、幂等、游标分页 |
| db-migration-safe | 零停机迁移：expand→migrate→contract + 锁表风险评估 |
| dep-guardian | 风险分级依赖升级 + 新依赖准入审查 |
| legacy-explainer | 数据流式代码解释，结论带 file:line 出处 |

**Pro 版还包含：**
- 📖 **《Claude Code 中文实战手册》（11 章）**——黄金工作流组合（含实战对话示例）、10 个让产出翻倍的提问方式、8 条高频坑解法、场景速查表、**AI 代码八类缺陷核对清单**、**如何写你自己的 Skill（description 公式 + 发布前三测）**、团队三周落地路线
- ⚡ 一键安装脚本（bash + PowerShell）
- 🏢 团队部署方案（提交进仓库 `.claude/skills/`，全团队规范自动对齐）
- 🔄 后续版本更新

### 获取 Pro 包 / Get the Pro Pack

🚚 自动支付上线准备中——**点个 ⭐ 并[订阅发布](../../releases)**，上线当天会收到通知。**现在就能买**：在 [issue #1](../../issues/1) 评论「购买 + 中文/英文」，24 小时内回复付款方式并发货（¥19.9 / $9.9）。

🚀 Shipping now — **star the repo and watch [releases](../../releases)** to get notified. **Buy today**: comment "buy + CN/EN" in [issue #1](../../issues/1) and we reply with payment options and deliver within 24h (¥19.9 / $9.9).

---

## 为什么是 skills 而不是提示词？

提示词是"每次重复交代"；skill 是"交代一次，永久生效"。
Skills trigger automatically based on what you're doing — describe the intent ("review my changes"), get the workflow ("seven-dimension review, findings ranked, no rubber-stamping").

## License

- 本仓库免费样品：MIT
- Pro 包内容：购买者可永久使用，详见商品页
