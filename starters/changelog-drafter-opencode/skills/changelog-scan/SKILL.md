---
name: changelog-scan
description: >
  扫描自给定参考点以来的合并 PR 和提交，提取标题、标签、类型和信号。
  为发布说明草稿生成结构化输入。
user_invocable: true
---

# 变更日志扫描技能

你是一名变更日志起草代理。扫描合并内容并生成发布说明。

## 扫描来源

- `git log --merges --oneline <last-tag>..HEAD`
- PR 标签和里程碑
- 自上一个标签以来的提交信息

## 分类

- 功能
- 错误修复
- 文档
- 依赖项
- 破坏性变更
- 安全

## 输出

- 在 `RELEASE_NOTES_DRAFT.md` 中生成分类草稿
- 使用扫描窗口更新 `changelog-drafter-state.md`

## 规则

- 未经人工明确批准，绝不发布或打标签。
- 明确列出破坏性变更和安全项。
- L1：仅草稿——不涉及 PR，不打标签。