# 最小循环启动器 — Claude Code

将此内容克隆到你的项目根目录，即可运行一个**仅报告模式的每日分类循环**（L1 就绪状态）。

## 快速开始

1. 将文件复制到你的仓库中：

   ```bash
   cp -r starters/minimal-loop-claude/.claude/skills/loop-triage .claude/skills/
   cp starters/minimal-loop-claude/.claude/agents/loop-verifier.md .claude/agents/
   cp starters/minimal-loop-claude/STATE.md.example STATE.md
   cp starters/minimal-loop-claude/LOOP.md .
   ```

2. 自定义 `STATE.md` 中的项目名称。

3. 启动循环（Claude Code）：

   ```bash
   /loop 1d Run $loop-triage. Read STATE.md first. Append high-priority and watch items. Update Last run timestamp. Do not auto-fix anything in week one.
   ```

4. 每天早上阅读 `STATE.md`，持续 1–2 周。调整分类技能。

5. 当分类质量良好时，从 `templates/SKILL.md.minimal-fix` 添加 `minimal-fix`，并通过验证代理（对实现任务使用 `isolation: worktree`）启用小型自动修复功能。

## 包含内容

| 文件 | 用途 |
|------|---------|
| `STATE.md.example` | 状态主干模板 |
| `.claude/skills/loop-triage/SKILL.md` | 分类技能 |
| `.claude/agents/loop-verifier.md` | 用于 L2+ 的检查子代理 |
| `LOOP.md` | 团队循环配置文档 |

## 后续步骤

- [循环设计清单](../../docs/loop-design-checklist.md)
- [每日分类模式](../../patterns/daily-triage.md)
- [Claude Code 示例](../../examples/claude-code/daily-triage.md)
- 运行 `npx @cobusgreyling/loop-audit .` 获取就绪评分