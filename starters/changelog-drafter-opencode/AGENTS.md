# AGENTS.md — 变更日志起草器（Opencode）

## 循环模式
- 前 1-2 周执行 L1 报告与草稿生成。
- 每次运行前读取 `changelog-drafter-state.md`。
- 任何发布或 Pull Request 前，草稿均需人工批准。

## 安全
- 未经明确人工批准，不得创建标签、GitHub 发布版本或更新 CHANGELOG.md。
- 破坏性变更和安全事项必须明确呈现。
- 仅在启用 L2 时创建草稿 Pull Request——绝不自动合并。

## 验证
- 验证器检查草稿完整性：分类、日期范围、链接。
- 在状态文件中记录草稿状态和人工反馈。