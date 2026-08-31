# Issue 分类 — Opencode 入门模板

即克隆即运行的脚手架，用于 opencode 的 **Issue 分类**模式。保持 Issue 队列清晰可读，让每日分类工作始终能快速定位前五项。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern issue-triage --tool opencode
```

或手动复制：

```bash
cp -r starters/issue-triage-opencode/skills .
cp starters/issue-triage-opencode/AGENTS.md .
cp starters/issue-triage-opencode/LOOP.md .
cp starters/issue-triage-opencode/issue-triage-state.md.example issue-triage-state.md
cp starters/issue-triage-opencode/opencode.json.example opencode.json
```

启动（L1 仅提议模式，第 1 周）：

```bash
opencode run \
  "运行 skills/issue-triage/SKILL.md。扫描自上次运行以来的开放 Issue。使用前 5 项、建议标签和需人工处理分类更新 issue-triage-state.md。仅提议——不应用标签或关闭 Issue。" \
  --title "Issue 分类"
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `issue-triage-state.md.example` | 状态主干（Issue 队列健康度） |
| `skills/issue-triage/SKILL.md` | 聚焦 Issue 的分类技能 |
| `AGENTS.md` | 常驻安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/issue-triage.md](../../patterns/issue-triage.md)
- [examples/opencode/issue-triage.md](../../examples/opencode/issue-triage.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)