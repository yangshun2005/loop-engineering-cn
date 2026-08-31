# 合并后清理 — Opencode 入门模板

用于 **合并后清理** 模式的克隆即运行脚手架，配合 opencode 使用。在合并到 main 分支后跟进技术债务。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern post-merge-cleanup --tool opencode
```

或手动复制：

```bash
cp -r starters/post-merge-cleanup-opencode/skills .
cp starters/post-merge-cleanup-opencode/AGENTS.md .
cp starters/post-merge-cleanup-opencode/LOOP.md .
cp starters/post-merge-cleanup-opencode/post-merge-state.md.example post-merge-state.md
cp starters/post-merge-cleanup-opencode/opencode.json.example opencode.json
```

开始（仅报告模式，第 1 周）：

```bash
opencode run \
  "运行 skills/post-merge-scan/SKILL.md。扫描最近 48 小时内合并到 main 的变更。仅提出小的文档/代码风格/注释修复建议 — 架构性问题请提交 Issue。更新 post-merge-state.md。不要修改源代码。" \
  --title "合并后清理"
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `post-merge-state.md.example` | 状态主干模板 |
| `skills/post-merge-scan/SKILL.md` | 聚焦合并的扫描技能 |
| `AGENTS.md` | 始终生效的安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/post-merge-cleanup.md](../../patterns/post-merge-cleanup.md)
- [examples/opencode/post-merge-cleanup.md](../../examples/opencode/post-merge-cleanup.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)
- [stories/post-merge-cleanup-honest-win.md](../../stories/post-merge-cleanup-honest-win.md)