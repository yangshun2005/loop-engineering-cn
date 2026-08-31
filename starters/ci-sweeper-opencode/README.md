# CI Sweeper — Opencode 入门模板

基于 opencode 的 **CI Sweeper** 模式的克隆即用脚手架。以短周期响应失败的 CI，进行最小修复。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern ci-sweeper --tool opencode
```

或手动复制：

```bash
cp -r starters/ci-sweeper-opencode/skills .
cp starters/ci-sweeper-opencode/AGENTS.md .
cp starters/ci-sweeper-opencode/LOOP.md .
cp starters/ci-sweeper-opencode/ci-sweeper-state.md.example ci-sweeper-state.md
cp starters/ci-sweeper-opencode/opencode.json.example opencode.json
```

开始（L2 谨慎模式——CI sweeper 从第一周起即面向行动）：

```bash
opencode run \
  "运行 skills/ci-triage/SKILL.md。对每个失败的检查进行分类。对于明确的单文件回归，创建工作树并应用带验证器的最小修复。在提交前运行 loop-gate 检查。更新 ci-sweeper-state.md。基础设施和安全失败：升级处理。" \
  --title "CI sweeper"
```

## 包含内容

| 文件 | 用途 |
|------|------|
| `ci-sweeper-state.md.example` | 状态主干（进行中的 CI 失败） |
| `skills/ci-triage/SKILL.md` | 面向 CI 的故障分类技能 |
| `AGENTS.md` | 常驻安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/ci-sweeper.md](../../patterns/ci-sweeper.md)
- [examples/opencode/ci-sweeper.md](../../examples/opencode/ci-sweeper.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)