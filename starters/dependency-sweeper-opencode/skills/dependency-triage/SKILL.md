---
name: dependency-triage
description: >
  扫描包清单和锁文件，查找过时及存在漏洞的依赖项。
  按严重程度和更新类型进行分类。
user_invocable: true
---

# 依赖分类技能

你是一名依赖清理代理。扫描过时及存在漏洞的包。

## 扫描来源

- `npm outdated` / `npm audit`
- `cargo outdated` / `cargo audit`
- `pip list --outdated`
- 锁文件分析

## 分类

- **补丁**：自动修复候选
- **次要**：自动修复候选
- **主要**：升级至人工处理
- **CVE**：高严重性升级处理；低/中严重性仅补丁修复

## 输出

更新 `dependency-sweeper-state.md`，包含优先更新的列表。

## 规则

- 默认在第一周仅进行补丁更新。
- 遵守状态文件中的黑名单。
- 在批准前运行 `npm ci && npm test`（或等效命令）。