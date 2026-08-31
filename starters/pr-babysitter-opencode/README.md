# PR 保姆 — Opencode 入门模板

基于 opencode 的 **PR 保姆** 模式的克隆即运行脚手架。监控开放的 PR，识别阻塞问题，并仅执行安全且显而易见的维护任务。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern pr-babysitter --tool opencode
```

或者手动复制：

```bash
cp -r starters/pr-babysitter-opencode/skills .
cp starters/pr-babysitter-opencode/AGENTS.md .
cp starters/pr-babysitter-opencode/LOOP.md .
cp starters/pr-babysitter-opencode/pr-babysitter-state.md.example pr-babysitter-state.md
cp starters/pr-babysitter-opencode/opencode.json.example opencode.json
```

开始（仅报告，第一周）：

```bash
opencode run \
  "运行 PR 保姆分诊。首先读取 pr-babysitter-state.md。列出 CI 为红色、审查过期、存在合并冲突或审查评论未回复的 PR。不要编辑代码。更新 pr-babysitter-state.md。" \
  --title "PR 保姆"
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `pr-babysitter-state.md.example` | 状态主干模板 |
| `skills/pr-review-triage/SKILL.md` | 面向 PR 的分诊技能 |
| `AGENTS.md` | 始终生效的安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/pr-babysitter.md](../../patterns/pr-babysitter.md)
- [examples/opencode/pr-babysitter.md](../../examples/opencode/pr-babysitter.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)
- [stories/pr-babysitter-week-one.md](../../stories/pr-babysitter-week-one.md)