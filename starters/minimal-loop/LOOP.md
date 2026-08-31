# 循环配置 — 最小化分诊

## 活动循环

| 模式 | 节奏 | 状态 | 命令 |
|---------|---------|--------|---------|
| 每日分诊 | 1天 | L1 仅报告 | 参见 README |

## 人工门禁

- 在 L2 检查清单完成前，不进行自动修复
- 所有高风险路径：必须经过人工审核

## 预算

- 每次运行的最大子代理生成数：0（L1）/ 2（L2）
- 每日最大令牌数：100k（参见 `loop-budget.md`）
- 每次运行追加至 `loop-run-log.md`；在开始和结束时使用 `loop-budget` 技能
- 终止开关：`loop-pause-all` — 暂停调度器并通知人工
- 估算：`npx @cobusgreyling/loop-cost --pattern daily-triage`

## 链接

- 模式：[每日分诊](../../patterns/daily-triage.md)
- 检查清单：[循环设计检查清单](../../docs/loop-design-checklist.md)