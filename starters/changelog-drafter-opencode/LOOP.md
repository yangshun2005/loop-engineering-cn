# 循环配置 — 变更日志起草器（Opencode）

## 活动循环

| 模式 | 节奏 | 状态 | 命令 |
|---------|---------|--------|---------|
| 变更日志起草器 | 1天或发布准备时 | L1 仅草稿 | `opencode run "运行 changelog-scan"` 通过 cron（在合并稳定后） |

## 人工审批关卡
- 破坏性变更始终需要人工审查。
- 安全说明 / CVE 的措辞必须由人工批准。
- 未经明确批准，不得打标签、提交 Pull Request 或发布。
- 前 3 次发布：人工批准完整草稿。

## 预算
- 每次运行最多生成子代理数量：2（扫描器 + 起草器）。
- 建议在合并稳定后运行（晚间）。

## 输出约定
- 草稿：仓库根目录下的 `RELEASE_NOTES_DRAFT.md`。
- 状态：`changelog-drafter-state.md`。

## 链接
- 模式：[changelog-drafter](../../patterns/changelog-drafter.md)
- 示例：[examples/opencode/changelog-drafter.md](../../examples/opencode/changelog-drafter.md)
- 安全：[docs/safety.md](../../docs/safety.md)