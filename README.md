

> **[中文化] loop-engineering**
>
> 此项目是 [cobusgreyling/loop-engineering](https://github.com/cobusgreyling/loop-engineering) 的中文翻译版本。
> - 原项目 Stars: 0
> - 主语言: 
> - 许可证: 
> - 翻译日期: 2026-08-31
> - 原始 README: [README_en.md](README_en.md)
>
> 如有翻译不准确之处，欢迎提 Issue 或 PR。

---


---

# Loop Engineering

<p align="center">
  <a href="https://cobusgreyling.github.io/loop-engineering/"><img src="https://img.shields.io/badge/✨_Showcase-live-3ee8c5?style=for-the-badge&labelColor=111a28" alt="Showcase" /></a>
  <a href="https://github.com/cobusgreyling/loop-engineering/stargazers"><img src="https://img.shields.io/github/stars/cobusgreyling/loop-engineering?style=social" alt="GitHub stars"></a>
  <a href="https://www.npmjs.com/package/@cobusgreyling/loop"><img src="https://img.shields.io/npm/v/@cobusgreyling/loop?label=loop" alt="loop npm"></a>
  <a href="https://raw.githubusercontent.com/cobusgreyling/loop-engineering/main/docs/last-run.json"><img src="https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/cobusgreyling/loop-engineering/main/docs/last-run.json" alt="last triage"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT"></a>
</p>

<p align="center">
  <a href="https://cobusgreyling.github.io/loop-engineering/">
    <img src="https://cdn.jsdelivr.net/gh/cobusgreyling/loop-engineering@main/assets/visuals/loop-engineering-logo.svg" alt="Loop Engineering logo" width="72" />
  </a>
</p>

> **停止提示。设计循环。获得评分。**

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/cobusgreyling/loop-engineering@main/assets/visuals/LE5.jpeg" alt="Loop Engineering — 设计驱动你的 Agent 的系统" width="100%" />
</p>

<p align="center"><strong>5 分钟快速上手</strong> ·
  <a href="docs/QUICKSTART.md">快速入门</a> ·
  <a href="docs/jobs.md">你想做什么？</a> ·
  <a href="docs/refactor.md">重构路径</a> ·
  <a href="README.zh-CN.md">中文</a> ·
  <a href="https://cobusgreyling.github.io/loop-engineering/#interactive">模式选择器</a>
</p>

```bash
npx @cobusgreyling/loop init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop doctor .
```

如果省略 `--tool`，默认值为 `claude`。可替换为 `grok`、`codex` 或 `opencode`。第一周为**仅报告**模式。

<p align="center">
  <a href="docs/QUICKSTART.md">
    <img src="https://cdn.jsdelivr.net/gh/cobusgreyling/loop-engineering@main/assets/visuals/loop-audit-demo.gif" alt="Loop Ready 评分攀升" width="100%" />
  </a>
</p>

你设计一个系统，让它发现工作、将工作交给 Agent、验证结果并持久化状态——而不是自己输入下一个提示。

**初次接触？** [快速入门](docs/QUICKSTART.md) · [重构项目](docs/refactor.md) · [精简 GitHub Action 循环](starters/thin-loop/)（无需 `STATE.md`）

## 你想做什么？

这是一个**围绕代码库运行 Agent 的模式库**。它不是“重写模块”的按钮。

| 我想…… | 从这里开始 |
|------------|------------|
| 保持仓库健康（Issue、CI、依赖） | [快速入门](docs/QUICKSTART.md) → `daily-triage` |
| 落地并照看 PR | [PR 保姆](patterns/pr-babysitter.md) |
| 在 GitHub Actions 中用极少的文件运行循环 | [精简循环](starters/thin-loop/) |
| **交付功能或进行重构**（待办事项 → 小 PR） | **[重构 / 变更路径](docs/refactor.md)** |

完整任务表：[docs/jobs.md](docs/jobs.md)。

## 模式

| 模式 | 节奏 | 第 1 周 | 成本 |
|---------|---------|--------|------|
| [每日分诊](patterns/daily-triage.md) | 1 天–2 小时 | L1 报告 | 低 |
| [精简循环](patterns/thin-loop.md) | 事件 + 1 天 | L1 快照 | 极低 |
| [PR 保姆](patterns/pr-babysitter.md) | 5–15 分钟 | L1 观察 | 高 |
| [CI 清扫器](patterns/ci-sweeper.md) | 5–15 分钟 | L2 谨慎 | 极高 |
| [依赖清扫器](patterns/dependency-sweeper.md) | 6 小时–1 天 | L2 仅补丁 | 中 |
| [变更日志起草器](patterns/changelog-drafter.md) | 1 天或打标签 | L1 草稿 | 低 |
| [合并后清理](patterns/post-merge-cleanup.md) | 1 天–6 小时 | L1 非高峰 | 低 |
| [Issue 分诊](patterns/issue-triage.md) | 2 小时–1 天 | L1 仅建议 | 低 |

交互式选择器：[展示页](https://cobusgreyling.github.io/loop-engineering/#interactive)。索引：[patterns/registry.yaml](patterns/registry.yaml)。

## 快速开始

```bash
npx @cobusgreyling/loop init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop doctor .
npx @cobusgreyling/loop cost --pattern daily-triage --level L1
```

空仓库 → 首次 `STATE.md` 演练：[scripts/empty-to-state-demo.sh](scripts/empty-to-state-demo.sh)。

只有在验证器连续一周表现正确后，才逐步推出 **L1 报告 → L2 辅助 → L3 无人值守**。参见 [loop-design-checklist](docs/loop-design-checklist.md)。

Loop Ready 现在**对近期运行的加权高于磁盘上的文件**。30 天前的 `STATE.md` 不视为 L3。

## 按工具分类的示例

[Claude Code](examples/claude-code/)（包括[插件](examples/claude-code/plugin.md)）· [Grok](examples/grok/daily-triage.md) · [Codex](examples/codex/) · [OpenClaw](examples/openclaw/daily-triage.md) · [Opencode](examples/opencode/) · [GitHub Actions](examples/github-actions/)

## 运维与安全

[故障模式](docs/failure-modes.md) · [反模式](docs/anti-patterns.md) · [安全](docs/safety.md) · [循环运维](docs/operating-loops.md) · [案例故事](stories/)（成功**和**失败）

Loop Engineering 会放大判断力。Token 成本可能激增。无人值守的循环会犯无人值守的错误。请阅读循环产出的内容。

## 寻求帮助

文档和小型 PR：**48 小时内**首次响应。

- [添加你的项目](https://github.com/cobusgreyling/loop-engineering/issues/new?template=add-adopter.yml)到[采用者列表](docs/adopters.md)
- [分享案例故事](https://github.com/cobusgreyling/loop-engineering/issues/new?template=share-story.yml)（失败同样重要）
- 实时[`good first issue`](https://github.com/cobusgreyling/loop-engineering/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)列表
- [展示你的循环](https://github.com/cobusgreyling/loop-engineering/discussions/326) · [提问交流](https://github.com/cobusgreyling/loop-engineering/discussions/327)

参见 [CONTRIBUTING.md](CONTRIBUTING.md)。

<details>
<summary>CLI 包和配套仓库（可选——第一周不需要）</summary>

统一入口：`npx @cobusgreyling/loop`（`init` · `doctor` · `status` · `audit` · `cost`）。旧包（`loop-init`、`loop-audit` 等）仍受支持。

后续阶段有配套工具：[memory-engineering](https://github.com/cobusgreyling/memory-engineering)、[harness-foundry](https://github.com/cobusgreyling/harness-foundry)、[outerloop](https://github.com/cobusgreyling/outerloop)、[fleet-engineering](https://github.com/cobusgreyling/fleet-engineering)、[goal-engineering](https://github.com/cobusgreyling/goal-engineering)。在循环实际运行之前，不要添加它们。

</details>

## 来源

- [Cobus Greyling — Loop Engineering](https://cobusgreyling.substack.com/p/loop-engineering)
- [Addy Osmani — Loop Engineering](https://addyosmani.com/blog/loop-engineering/)
- [Lulla et al. 2026 — Building Blocks, Adoption, and Impact](https://arxiv.org/abs/2608.21884)（本仓库是他们评审的社区参考实现）
- [致谢](resources/sources.md)

## 许可证

MIT