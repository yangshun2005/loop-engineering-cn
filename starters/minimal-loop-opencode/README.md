# 极简循环入门 — Opencode

将此目录克隆到你的项目根目录，即可使用 opencode 运行一个**仅报告模式的每日分流循环**（L1 就绪状态）。

## 快速开始

1. 脚手架搭建（推荐）：

   ```bash
   npx @cobusgreyling/loop-init . --pattern daily-triage --tool opencode
   ```

   或手动复制：

   ```bash
   cp -r starters/minimal-loop-opencode/skills .
   cp starters/minimal-loop-opencode/AGENTS.md .
   cp starters/minimal-loop-opencode/LOOP.md .
   cp starters/minimal-loop-opencode/STATE.md.example STATE.md
   cp starters/minimal-loop-opencode/opencode.json.example opencode.json
   ```

2. 自定义 `STATE.md` 中的项目名称。

3. 启动循环：

   ```bash
   opencode run "运行 loop-triage。首先阅读 STATE.md。添加高优先级和关注项。更新上次运行时间戳。第一周内不要自动修复任何问题。" --agent loop-triage
   ```

4. 每天早上阅读 `STATE.md`，持续 1-2 周。调整分流技能。

5. 当分流质量良好时，从 `templates/SKILL.md.minimal-fix` 添加 `minimal-fix`，并在工作树中使用验证器代理启用小型自动修复。

## 包含内容

| 文件 | 用途 |
|------|---------|
| `STATE.md.example` | 状态主干模板 |
| `skills/loop-triage/SKILL.md` | 分流技能 |
| `AGENTS.md` | opencode 的常驻项目规则 |
| `LOOP.md` | 供团队参考的循环配置文档 |
| `opencode.json.example` | opencode 代理定义示例 |

## 后续步骤

- [循环设计清单](../../docs/loop-design-checklist.md)
- [每日分流模式](../../patterns/daily-triage.md)
- [Opencode 示例](../../examples/opencode/daily-triage.md)
- 运行 `npx @cobusgreyling/loop-audit .` 获取就绪评分