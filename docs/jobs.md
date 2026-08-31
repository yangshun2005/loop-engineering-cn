# 你想做什么？

本仓库是一个**围绕代码库运行代理的模式库**——用于问题分类、照看 Pull Request、清理 CI、保持依赖最新。它不是一个“帮我完成功能 / 重写模块”的按钮。

| 我想…… | 从这里开始 | **不要**从这里开始 |
|------------|------------|------------------------|
| 保持仓库健康（Issue、CI、依赖、变更日志） | [快速入门](./QUICKSTART.md) → `daily-triage` | 第一天就无人值守运行 L3 |
| 以几乎零文件的方式将循环作为单个 GitHub Action 运行 | [精简循环](../starters/thin-loop/) | 期望获得很高的 Loop Ready 评分 |
| 落地并照看 Pull Request | [PR 照看者](../patterns/pr-babysitter.md) | 从循环中直接合并 |
| 修复构建失败 | [CI 清理者](../patterns/ci-sweeper.md) | 永远在修补偶发问题 |
| 安全地升级依赖 | [依赖清理者](../patterns/dependency-sweeper.md) | 在没有人工参与的情况下进行大版本升级 |
| **交付功能或进行重构**（将工作拆分为待办事项并执行） | **[重构 / 变更路径](./refactor.md)** | 一个单一的“重构仓库”循环 |
| 运行直到完成一个明确的目标 | [目标工程](https://github.com/cobusgreyling/goal-engineering) (`/goal`) | 假装 `daily-triage` 是一个任务运行器 |
| 跨会话持久化记忆 | [记忆工程](https://github.com/cobusgreyling/memory-engineering) | 把所有内容都塞进 `STATE.md` |
| 管理多个代理 | [舰队工程](https://github.com/cobusgreyling/fleet-engineering) | 为整个组织使用一个 `LOOP.md` |

**第一周规则仍然适用：** 仅报告。在让循环执行操作之前，先阅读它写入的内容。

```bash
npx @cobusgreyling/loop init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop doctor .
```

`--tool` 默认为 `claude`。可替换为 `grok`、`codex` 或 `opencode`。不确定该选哪个模式？[模式选择器](https://cobusgreyling.github.io/loop-engineering/#interactive)。