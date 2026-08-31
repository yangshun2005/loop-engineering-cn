# 精简循环

**目标：** 仅使用 GitHub Actions 运行一个仅报告循环，几乎不需要额外文件。Issue 跟踪器和 Actions 作业摘要*就是*状态。

此模式的存在是因为现实中大多数循环已经呈现这种形态。[Lulla 等人 (2026)](https://arxiv.org/abs/2608.21884) 在 36,000 个仓库中确认了 217 个自主代理循环：它们提交**触发器**（cron / `pull_request` 工作流），并且几乎从不提交 `STATE.md`。精简循环有意匹配这种形态。

## 何时使用

- 您希望本周就有一个循环，而不是安装一套方法论。
- PR 审查或 Issue 快照就是任务；没有需要持久化而 GitHub 尚未存储的内容。
- 如果发现结果需要在多次运行中保留（积压事项、尝试次数、预算），您**稍后**会添加 `STATE.md`。

如果您需要尝试次数上限、制造者/检查者分离或令牌紧急停止开关，请**不要**从这里开始——请使用[每日分流](./daily-triage.md)或[PR 保姆](./pr-babysitter.md)。

## 调度

| 平台 | 触发器 |
|---------|---------|
| GitHub Actions | `issues` / `pull_request` 打开 + 工作日 cron（参见入门模板） |
| Claude / Grok / Codex | 可选后续：`/loop 1d` 读取上次作业摘要 |

第一周：**L1**。工作流写入 `$GITHUB_STEP_SUMMARY`。仅当触发线程上尚不存在精简循环标记时，它才在触发的 Issue/PR 上评论。

## 所需技能

L1 级别无需任何技能。工作流仅使用 `gh`。

- `loop-triage` — 可选，如果您稍后通过 [loop-action](../tools/loop-action/) 将代理接入 Action
- `loop-verifier` — 可选，在任何写入之前必需；一旦存在实现者，制造者/检查者仍然适用

状态文件：**不需要**。GitHub Issues/PRs 承载积压事项。`loop-run-log.md` 行是 Loop Ready 活动评分的可选证明。

## 典型周期

1. 事件或工作日 cron 触发。
2. 工作流列出打开的 PR 和 Issues（`gh`）。
3. 快照进入作业摘要。
4. 如果事件是 Issue 或 PR，则发布一次简短的 L1 评论。
5. 人工阅读摘要或评论。无代码编辑，无自动合并。

## 验证策略

没有实现者，因此没有验证者。检查是机械性的：`gh` 成功且摘要非空。

如果您稍后调用代理，请在**任何写入之前**添加 [loop-verifier](../templates/SKILL.md.verifier)——制造者/检查者，而不是同一会话自我评分。

## 人工交接

一切都是交接。循环不会关闭 Issues、应用标签或推送提交。

## 工具说明

- **GitHub Actions** 是原生运行时。入门模板：[starters/thin-loop](../starters/thin-loop/)。
- 其他工具：使用 `npx @cobusgreyling/loop init . --pattern thin-loop --tool claude` 搭建脚手架（工具后缀被忽略；您仍然会获得工作流）。

## 失败模式

| 失败 | 应对措施 |
|---------|------------|
| 评论垃圾信息 | 标记 `<!-- thin-loop -->` 跳过第二次评论 |
| 空快照 | `gh` 认证——工作流需要 `issues: write` / `pull-requests: write` |
| 人们期望高 Loop Ready 分数 | 精简循环保持低分，直到存在带日期的运行日志或 `STATE.md` 上次运行记录。这是有意为之。 |

## 成功指标

- 合并后 10 分钟内首次绿色工作流运行。
- 在已有标记的线程上无评论。
- 循环本身对 `main` 的零文件编辑。