# 循环配置 — 最小分流（Opencode）

## 活跃循环

| 模式 | 节奏 | 状态 | 命令 |
|---------|---------|--------|---------|
| 每日分流 | 1天 | L1 仅报告 | `opencode run "Run loop-triage" --agent loop-triage` 通过 cron/systemd 执行 |

## 人工门禁

- 在 L2 检查清单完成之前，不进行自动修复。
- 所有高风险路径均需人工审查（参见 docs/safety.md 中的拒绝列表）。

## 工作树

- 使用显式的 `git worktree`，并在执行实现任务（L2 及以上）时使用 `--dir <worktree>` 运行 opencode。
- 每次修复尝试使用一个工作树；验证者拒绝后立即丢弃。

## 连接器（MCP）

- MCP 对于 L1 仅报告循环为可选。
- 对于 L2 及以上：GitHub MCP 可读取 CI/Issue；在受信任之前，将连接器范围限制为读取和评论。

## 预算

- 每次运行的最大子代理生成数：0（L1）。
- 每日查看 STATE.md。
- 如果令牌消耗达到每日上限的 80%，则切换为仅报告模式。

## 链接

- 模式：[每日分流](../../patterns/daily-triage.md)
- 检查清单：[循环设计检查清单](../../docs/loop-design-checklist.md)