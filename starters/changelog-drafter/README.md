# 变更日志起草器 Starter

用于[变更日志起草器](../../patterns/changelog-drafter.md)循环（L1 报告 + 草稿 → L2 辅助发布准备）的脚手架。这是一个低风险、适合作为第二个或第三个循环的优秀选择。

## 快速开始

```bash
# 推荐方式
npx @cobusgreyling/loop-init . --pattern changelog-drafter --tool grok

# 或手动复制（Grok）
cp -r starters/changelog-drafter/.grok/skills/changelog-scan .grok/skills/
cp -r starters/changelog-drafter/.grok/skills/draft-release-notes .grok/skills/
cp starters/changelog-drafter/changelog-drafter-state.md.example changelog-drafter-state.md
cp starters/changelog-drafter/LOOP.md .
```

开始使用（Grok 示例 — 第一周仅报告/草稿）：

```bash
/loop 1d 对自上次标签（或最近 7 天）以来的合并执行 changelog-scan。使用 draft-release-notes 技能在 RELEASE_NOTES_DRAFT.md 中生成一份清晰分类的草稿。更新 changelog-drafter-state.md。未经明确的人工批准，绝不发布或打标签。
```

## 文件

| 文件 | 用途 |
|------|---------|
| `changelog-drafter-state.md.example` | 跟踪上次发布、扫描窗口、待处理的草稿 |
| `.grok/skills/changelog-scan/` | 合并及信号的发现/分类 |
| `.grok/skills/draft-release-notes/` | 将结构化列表转化为精炼的用户可见说明 |
| `LOOP.md` | 团队的节奏、门禁和预算 |

## 建议同时引入（推荐）

- `loop-verifier`（或使用人工作为 L1 的验证者）
- 你项目的发布风格/风格指南（在 AGENTS.md 或项目技能中添加简短章节）

## 安全性

- 此循环仅提出草稿。发布（更新 CHANGELOG.md、创建 GitHub 发布版本、推送标签）应要求人工审核或使用非常严格的允许列表 + 验证者。
- 始终明确呈现破坏性变更和安全事项以供人工签署确认。
- 完整指南请参阅[safety.md](../../docs/safety.md)和模式文档。

复制完成后，运行：

```bash
npx @cobusgreyling/loop-audit . --suggest
```