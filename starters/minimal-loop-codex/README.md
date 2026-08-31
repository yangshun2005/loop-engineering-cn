# 最小循环入门 — Codex

将此内容克隆到你的项目根目录，即可运行一个**仅报告模式的每日分流循环**（L1 就绪状态）。

## 快速开始

1. 将文件复制到你的仓库中：

   ```bash
   cp -r starters/minimal-loop-codex/.codex/skills/loop-triage .codex/skills/
   cp starters/minimal-loop-codex/.codex/agents/verifier.toml .codex/agents/
   cp starters/minimal-loop-codex/STATE.md.example STATE.md
   cp starters/minimal-loop-codex/LOOP.md .
   ```

2. 自定义 `STATE.md` 中的项目名称。

3. 在 Codex 应用中创建一个**自动化**（自动操作选项卡）：

   | 字段 | 值 |
   |-------|--------|
   | 频率 | 每日（例如 `1d`） |
   | 环境 | 本地检出或后台工作树 |
   | 提示词 | 见下文 |

   ```
   运行 $loop-triage。先读取 STATE.md。追加高优先级和关注项。
   更新上次运行时间戳。第 1 周：仅报告 — 不要修改源文件。
   ```

4. 在 Codex 分流收件箱和 `STATE.md` 中查看结果，持续 1–2 周。

5. 当分流质量良好时，从 `templates/` 添加 `minimal-fix`，并在隔离的工作树中使用验证器子代理启用小型自动修复。

## 包含内容

| 文件 | 用途 |
|------|---------|
| `STATE.md.example` | 状态主干模板 |
| `.codex/skills/loop-triage/SKILL.md` | 分流技能 |
| `.codex/agents/verifier.toml` | 用于 L2+ 的检查子代理 |
| `LOOP.md` | 为你的团队准备的循环配置文档 |

## 后续步骤

- [循环设计检查清单](../../docs/loop-design-checklist.md)
- [每日分流模式](../../patterns/daily-triage.md)
- [Codex 示例](../../examples/codex/daily-triage.md)
- 运行 `npx @cobusgreyling/loop-audit .` 获取就绪评分