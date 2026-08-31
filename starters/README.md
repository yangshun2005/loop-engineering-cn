# 入门模板

克隆即用的脚手架。复制到你的项目中——或使用 `loop-init`：

```bash
npx @cobusgreyling/loop-init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop-init . -p thin-loop
npx @cobusgreyling/loop-init . -p pr-babysitter -t grok
```

## 每日分类（L1 仅报告）

| 模板 | 工具 | 路径 |
|---------|------|------|
| [minimal-loop](./minimal-loop/) | Grok | `.grok/skills/` |
| [minimal-loop-claude](./minimal-loop-claude/) | Claude Code | `.claude/skills/` + `.claude/agents/` |
| [minimal-loop-codex](./minimal-loop-codex/) | Codex | `.codex/skills/` + `.codex/agents/` |
| [minimal-loop-opencode](./minimal-loop-opencode/) | Opencode | `skills/` + `AGENTS.md` |
| [thin-loop](./thin-loop/) | GitHub Actions（任意工具） | 仅 `.github/workflows/thin-loop.yml` |

## L2 辅助模式

| 模板 | 模式 | 工具 | 就绪状态 |
|---------|---------|-------|-----------|
| [pr-babysitter](./pr-babysitter/) | PR 保姆 | Grok、Claude、Codex | L2 辅助 |
| [pr-babysitter-opencode](./pr-babysitter-opencode/) | PR 保姆 | Opencode | L1 → L2 |
| [ci-sweeper](./ci-sweeper/) | CI 清理器 | Grok、Claude、Codex | L2 辅助 |
| [ci-sweeper-opencode](./ci-sweeper-opencode/) | CI 清理器 | Opencode | L2 谨慎 |
| [dependency-sweeper](./dependency-sweeper/) | 依赖清理器 | Grok、Claude、Codex | L2 仅补丁 |
| [dependency-sweeper-opencode](./dependency-sweeper-opencode/) | 依赖清理器 | Opencode | L2 仅补丁 |
| [post-merge-cleanup](./post-merge-cleanup/) | 合并后清理 | Grok、Claude、Codex | L1 → L2 |
| [post-merge-cleanup-opencode](./post-merge-cleanup-opencode/) | 合并后清理 | Opencode | L1 → L2 |
| [changelog-drafter](./changelog-drafter/) | 变更日志起草器 | Grok、Claude、Codex | L1 草稿 → L2 |
| [changelog-drafter-opencode](./changelog-drafter-opencode/) | 变更日志起草器 | Opencode | L1 草稿 → L2 |
| [issue-triage](./issue-triage/) | Issue 分类 | Grok、Claude、Codex | L1 仅建议 |
| [issue-triage-opencode](./issue-triage-opencode/) | Issue 分类 | Opencode | L1 仅建议 |

复制后：

```bash
npx @cobusgreyling/loop-audit .
npx @cobusgreyling/loop-audit . --suggest
```