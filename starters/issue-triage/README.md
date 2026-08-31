# Issue 分类入门

[Issue 分类](../../patterns/issue-triage.md)循环的脚手架——L1 阶段仅提议，为期一周。与每日分类配合，作为低风险 Issue 队列的输入源。

## 快速开始

```bash
# Grok
npx @cobusgreyling/loop-init . --pattern issue-triage --tool grok

# Claude Code
npx @cobusgreyling/loop-init . --pattern issue-triage --tool claude

# Codex
npx @cobusgreyling/loop-init . --pattern issue-triage --tool codex
```

开始（Grok，第一周）：

```
/loop 2h 运行 issue-triage。先阅读 issue-triage-state.md。更新 Top 5 和建议标签。不自动应用。安全及模糊项需升级处理。
```

## 文件

| 文件 | 用途 |
|------|---------|
| `issue-triage-state.md.example` | 滚动积压健康状态 |
| `.grok/skills/issue-triage/` | Issue 扫描 + 优先级排序技能 |
| `.claude/agents/loop-verifier.md` | L2 标签应用检查器 |
| `LOOP.md` | 节奏与人工门禁 |

## 安全

- L1 阶段不自动打标签或关闭
- 黑名单：认证、支付、安全——参见 [safety.md](../../docs/safety.md)