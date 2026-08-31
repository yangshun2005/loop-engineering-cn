# 依赖清理器入门模板

用于**依赖清理器**模式（L2 辅助，仅补丁修复并带有强验证器门控）的克隆即运行脚手架。

## 快速开始

```bash
npx @cobusgreyling/loop-init . --pattern dependency-sweeper --tool grok
```

或手动复制：

```bash
cp -r starters/dependency-sweeper/.grok/skills/dependency-triage .grok/skills/
mkdir -p .grok/skills/loop-verifier .grok/skills/minimal-fix
cp templates/SKILL.md.verifier .grok/skills/loop-verifier/SKILL.md
cp templates/SKILL.md.minimal-fix .grok/skills/minimal-fix/SKILL.md
cp starters/dependency-sweeper/dependency-sweeper-state.md.example dependency-sweeper-state.md
cp starters/dependency-sweeper/LOOP.md .
```

Claude Code / Codex：使用 `loop-init` 时请指定 `--tool claude` 或 `--tool codex`。

开始（Grok）：

```
/loop 6h 对包清单和锁文件运行依赖分类。在工作树中仅进行补丁式自动修复 + 验证器（npm ci && npm test）。每次对同一包重试前运行 loop-context --check；提交前运行 loop-gate check。升级主版本、高危 CVE 和禁用列表中的包，在尝试 2 次后上报。更新 dependency-sweeper-state.md。
```

## 包含内容

| 文件 | 用途 |
|------|---------|
| `dependency-sweeper-state.md.example` | 状态主干（进行中的更新、禁用列表） |
| `.grok/.claude/.codex/skills/dependency-triage/` | 分类技能（所有工具） |
| `.claude/agents/loop-verifier.md` | 检查代理 |
| `.codex/agents/verifier.toml` | 检查子代理 |
| `LOOP.md` | 节奏、门控、预算 |

## 后续步骤

- [patterns/dependency-sweeper.md](../../patterns/dependency-sweeper.md)
- [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)
- [stories/dependency-sweeper-week-one.md](../../stories/dependency-sweeper-week-one.md)

**安全性**：主版本升级和高严重性破坏性修复始终保留在明确的人工门控之后。