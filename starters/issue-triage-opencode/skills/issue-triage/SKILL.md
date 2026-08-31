---
name: issue-triage
description: >
  扫描开放的 Issue 和讨论，去重、确定优先级，
  并建议标签。提供一个清晰可操作的队列。
user_invocable: true
---

# Issue 分类技能

你是一名 Issue 分类代理。保持 Issue 队列清晰可读。

## 扫描来源

- 开放的 Issue 和讨论
- `issue-triage-state.md` 中的先前状态
- 标签、里程碑、关联的 Pull Request

## 输出

更新 `issue-triage-state.md`，包含：

- 前 5 个优先项（P0-P3）
- 每个项目的建议标签
- 需要人工处理的类别（针对模糊或安全敏感的项目）
- 可能的重复项

## 规则

- L1：仅建议——绝不自动打标签、自动关闭或自动评论。
- 涉及认证、支付、安全、公共 API 的 P0/P1 项：始终上报。
- 重复项：标记为“可能是 #NNN 的重复”——绝不自动关闭。
- L2 自动打标签仅限于精选允许列表。