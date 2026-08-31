# 循环配置 — PR 保姆（Opencode）

## 活动循环

| 模式 | 频率 | 状态 | 命令 |
|------|------|------|------|
| PR 保姆 | 10-15 分钟 | L1 仅报告 | `opencode run "运行 PR 保姆分诊" --agent loop-triage` 通过 cron/systemd |

## 人工门禁
- 在 L2 检查清单完成前，不进行自动修复。
- 默认创建草稿 PR；由人工标记为可审查。
- 未经批准，不得解决审查线程。
- 安全、认证、支付、基础设施变更：始终由人工处理。

## 工作树
- 每次修复尝试使用一个工作树；验证器拒绝后丢弃。
- 使用 `--dir <工作树>` 运行实施器。

## 预算
- 每次运行的最大子代理生成数：0（L1）。
- 每个 PR 在移交前最多尝试 3 次修复。
- 超出令牌预算时暂停。

## 链接
- 模式：[pr-babysitter](../../patterns/pr-babysitter.md)
- 示例：[examples/opencode/pr-babysitter.md](../../examples/opencode/pr-babysitter.md)
- 安全：[docs/safety.md](../../docs/safety.md)