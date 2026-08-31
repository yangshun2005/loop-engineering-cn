# 循环配置 — Issue 分类（Opencode）

## 活动循环

| 模式 | 频率 | 状态 | 命令 |
|---------|---------|--------|---------|
| Issue 分类 | 2小时-1天 | L1 仅提议 | `opencode run "Run issue-triage" --agent loop-triage` 通过 cron 执行 |

## 人工审批关卡
- 涉及认证、支付、安全、公共 API 的 P0/P1 问题：始终需人工处理。
- 重复提议标记为“可能是 #NNN 的重复项”——绝不自动关闭。
- L2 自动标签仅限于允许列表。

## 预算
- 每次运行最大子代理生成数：0（L1），1（L2）。
- 若自上次运行以来没有新 Issue，则提前退出。

## 链接
- 模式：[issue-triage](../../patterns/issue-triage.md)
- 示例：[examples/opencode/issue-triage.md](../../examples/opencode/issue-triage.md)
- 安全：[docs/safety.md](../../docs/safety.md)