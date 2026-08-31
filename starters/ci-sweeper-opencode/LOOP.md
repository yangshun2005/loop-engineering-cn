# 循环配置 — CI 清扫器（Opencode）

## 活动循环

| 模式 | 频率 | 状态 | 命令 |
|---------|---------|--------|---------|
| CI 清扫器 | 5-15分钟 | L2 谨慎 | `opencode run "运行 ci-triage" --agent loop-triage` 通过 cron/systemd |

## 人工门禁
- 基础设施/安全/支付测试失败：不自动修复。
- 每个项目最多尝试修复 3 次；之后升级处理。
- 未经验证器通过，不得自动合并。

## 工作树
- 每次尝试务必分发到独立的 git 工作树中。
- 每次修复使用一个工作树；REJECT 后丢弃。

## 预算
- 每次运行最大子代理生成数：3。
- 每天最大自动 Pull Request 数：5。
- 若未发现失败，则提前退出。

## 链接
- 模式：[ci-sweeper](../../patterns/ci-sweeper.md)
- 示例：[examples/opencode/ci-sweeper.md](../../examples/opencode/ci-sweeper.md)
- 安全：[docs/safety.md](../../docs/safety.md)