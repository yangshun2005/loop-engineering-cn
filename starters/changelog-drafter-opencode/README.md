# 变更日志起草器 — Opencode 入门模板

这是一个即克隆即运行的脚手架，用于在 opencode 中实现**变更日志起草器**模式。扫描已合并的 PR 和提交，生成分类的发布说明。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern changelog-drafter --tool opencode
```

或手动复制：

```bash
cp -r starters/changelog-drafter-opencode/skills .
cp starters/changelog-drafter-opencode/AGENTS.md .
cp starters/changelog-drafter-opencode/LOOP.md .
cp starters/changelog-drafter-opencode/changelog-drafter-state.md.example changelog-drafter-state.md
cp starters/changelog-drafter-opencode/opencode.json.example opencode.json
```

启动（第 1 级报告 + 草稿，第 1 周）：

```bash
opencode run \
  "运行 skills/changelog-scan/SKILL.md。扫描自上次标签（或最近 7 天）以来的合并。在 RELEASE_NOTES_DRAFT.md 中生成分类草稿。更新状态。不要发布或创建 PR。" \
  --title "变更日志起草器"
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `changelog-drafter-state.md.example` | 状态主干（上次发布、草稿状态） |
| `skills/changelog-scan/SKILL.md` | 合并扫描与分类技能 |
| `AGENTS.md` | 常驻安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/changelog-drafter.md](../../patterns/changelog-drafter.md)
- [examples/opencode/changelog-drafter.md](../../examples/opencode/changelog-drafter.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)
- [stories/changelog-drafter-week-one.md](../../stories/changelog-drafter-week-one.md)