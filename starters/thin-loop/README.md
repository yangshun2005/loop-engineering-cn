# Thin Loop 入门模板

一个 GitHub Action。无需 `STATE.md`，无需技能，无需预算文件。

Issue 跟踪器是核心。任务摘要即报告。第一周为 L1（仅报告）。

## 安装

```bash
npx @cobusgreyling/loop init . --pattern thin-loop --tool claude
```

或直接复制：

```bash
mkdir -p .github/workflows
cp starters/thin-loop/.github/workflows/thin-loop.yml .github/workflows/
```

合并到 `main` 分支。打开一个 Issue 或等待工作日定时任务触发。阅读 Actions 摘要。

## 你将获得

| 文件 | 用途 |
|------|---------|
| `.github/workflows/thin-loop.yml` | 事件 + 工作日快照 |
| `LOOP.md` | 一页展示节奏与门禁 |

## 你（有意）不会获得的内容

- `STATE.md` — 当发现结果需要在多次运行间持久化时，请自行添加（参见 [每日分类](../minimal-loop-claude/)）。
- Agent 调用 — 若希望模型中参与循环，可稍后接入 [loop-action](../../tools/loop-action/)。
- 高 Loop Ready 评分 — 磁盘上的文件并非此处的目标。

## 第一周

1. 合并工作流。
2. 确认一次运行成功。
3. 阅读摘要。不要让它自动打标签、关闭或推送。
4. 仅当需要 GitHub 尚未提供的队列时，再升级到每日分类。