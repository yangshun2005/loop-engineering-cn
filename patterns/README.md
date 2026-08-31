# 循环模式

有文档记录的、可复用的循环模式，这些模式已在（或可在）真实环境中运行。

每种模式都回答了以下问题：
- 它解决了什么问题
- 推荐的调度方式
- 所需技能 / 状态结构
- 验证方法
- 人工交接策略
- 工具特定说明（Grok、Claude Code、Codex、OpenClaw、Opencode、GitHub Actions）

## 模式注册表

| 模式 | 节奏 | 风险 | 文件 |
|---------|---------|------|------|
| PR 保姆 | 5–15 分钟 | 中 | [pr-babysitter.md](./pr-babysitter.md) |
| 每日分诊 | 1 天–2 小时 | 低 | [daily-triage.md](./daily-triage.md) |
| Issue 分诊（新） | 2 小时–1 天 | 低 | [issue-triage.md](./issue-triage.md) |
| CI 清理器 | 5–15 分钟 | 中 | [ci-sweeper.md](./ci-sweeper.md) |
| 合并后清理 | 1 天–6 小时 | 低 | [post-merge-cleanup.md](./post-merge-cleanup.md) |
| 依赖清理器 | 6 小时–1 天 | 中 | [dependency-sweeper.md](./dependency-sweeper.md) |
| 变更日志起草器 | 1 天 | 低 | [changelog-drafter.md](./changelog-drafter.md) |
| 精简循环 | 事件 + 1 天 | 低 | [thin-loop.md](./thin-loop.md) |

机器可读索引：[registry.yaml](./registry.yaml)

## 如何使用模式

1. 选择一种模式：[pattern-picker.md](../docs/pattern-picker.md)
2. 使用 `npx @cobusgreyling/loop-init . --pattern <名称> --tool claude`（或 `--tool grok` / `--tool opencode`）搭建脚手架，或从 `starters/` 中复制
3. 如需超出起始模板进行自定义，可从 `templates/` 中复制技能
4. 设置调度（`/loop`、`scheduler_create`、GitHub Action、Codex Automation）
5. 在启用修复之前，第一周以 **L1 仅报告** 模式运行
6. 使用 `npx @cobusgreyling/loop-audit . --suggest` 进行审计

## 添加模式

使用 [templates/pattern-template.md](../templates/pattern-template.md)，在 `registry.yaml` 中添加条目，并提交 PR。请参阅 [CONTRIBUTING.md](../CONTRIBUTING.md)。