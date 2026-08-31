# 依赖清理器 — Opencode 入门模板

用于 **依赖清理器** 模式的克隆即运行脚手架，配合 opencode 使用。发现、安全应用并验证依赖更新。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern dependency-sweeper --tool opencode
```

或手动复制：

```bash
cp -r starters/dependency-sweeper-opencode/skills .
cp starters/dependency-sweeper-opencode/AGENTS.md .
cp starters/dependency-sweeper-opencode/LOOP.md .
cp starters/dependency-sweeper-opencode/dependency-sweeper-state.md.example dependency-sweeper-state.md
cp starters/dependency-sweeper-opencode/opencode.json.example opencode.json
```

启动（L2 辅助，仅补丁模式）：

```bash
opencode run \
  "运行 skills/dependency-triage/SKILL.md。扫描包清单以查找过时和易受攻击的依赖项。在工作树中使用验证器进行仅补丁自动修复。提交前运行循环门检查。升级主要版本、高危 CVE 和拒绝列表包。更新状态。" \
  --title "依赖清理器"
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `dependency-sweeper-state.md.example` | 状态主干（进行中的更新、拒绝列表） |
| `skills/dependency-triage/SKILL.md` | 面向依赖的分诊技能 |
| `AGENTS.md` | 始终在线的安全规则 |
| `LOOP.md` | 节奏、门禁、预算 |
| `opencode.json.example` | 命名代理定义 |

## 后续步骤

- [patterns/dependency-sweeper.md](../../patterns/dependency-sweeper.md)
- [examples/opencode/dependency-sweeper.md](../../examples/opencode/dependency-sweeper.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)
- [stories/dependency-sweeper-week-one.md](../../stories/dependency-sweeper-week-one.md)