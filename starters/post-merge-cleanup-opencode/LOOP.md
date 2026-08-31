# 循环配置 — 合并后清理（Opencode）

## 活动循环

| 模式 | 频率 | 状态 | 命令 |
|---------|---------|--------|---------|
| 合并后清理 | 1天 | L1 仅报告 | `opencode run "运行合并后扫描"` 通过 cron（非高峰时段） |

## 人工门禁
- 架构债务需创建工单，绝不自动修复。
- 黑名单路径始终由人工处理。
- 每次运行最多尝试修复 2 次。

## 工作树
- 任何代码更改尝试（L2 及以上）均使用 `git worktree`。
- 每个修复使用一个工作树；REJECT 后丢弃。

## 预算
- 每次运行的子代理生成上限：0（L1），2（L2）。
- 在非高峰时段（晚间）运行以避免冲突。
- 若超出令牌预算则暂停。

## 链接
- 模式：[post-merge-cleanup](../../patterns/post-merge-cleanup.md)
- 示例：[examples/opencode/post-merge-cleanup.md](../../examples/opencode/post-merge-cleanup.md)
- 安全：[docs/safety.md](../../docs/safety.md)