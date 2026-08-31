# 来源与致谢

本仓库的存在得益于资深从业者的公开讨论与实践。主要来源如下：

## 核心定义与框架

- **Cobus Greyling** — [循环工程](https://cobusgreyling.substack.com/p/loop-engineering)（2026年6月）
  本仓库的配套文章：上下文工程 vs 工具链工程 vs 循环工程、五大原语 + 记忆、Grok 映射以及生产实践现实。

- **Addy Osmani** — [循环工程](https://addyosmani.com/blog/loop-engineering/)（2026年6月）
  对该概念最完整的书面综合阐述，包括五大原语 + 记忆表以及具体的日常循环示例。

- 原始 X 帖子：https://x.com/addyosmani/status/2064127981161959567

## Anthropic / Claude Code 视角

- **Boris Cherny**（Anthropic 公司 Claude Code 负责人）
  - 公开表态：“我不再直接提示 Claude 了。我有循环在运行，它们会提示 Claude 并决定下一步做什么。我的工作就是编写循环。”
  - 社区中分享的实际用法：`/loop 5m /babysit`、`/loop 30m /slack-feedback`、`/loop /post-merge-sweeper`、`/pr-pruner` 等。
  - 强调将工作流转化为技能 + 循环。
  - 将 `/loop`、`/goal` 和 `/schedule` 作为 Claude Code 中的一等原语。

- Claude Code 团队（包括 @_catwu）的相关评论，强调循环是长期智能体工作中杠杆率最高的功能之一。

## 相关概念（Osmani）

- 智能体工具链工程
- 工厂模型
- 意图债务
- 理解债务
- 认知让渡
- 编排税
- 代码智能体乐团 / 对抗性代码审查

这些概念为循环设计提供了重要的背景和护栏。

## 其他早期信号

- Peter Steinberger：强调设计循环而非直接提示智能体。
- 多位从业者正在尝试闭环系统、反馈循环工程，以及“提示模型的例程”。

## 社区理念文档

- **Marius（[@sololys](https://github.com/sololys)）** — [KY 切割面理念 v0.1](./ky-cut-surface-philosophy-v0_1/)（2026年7月）
  关于生成与后果之区别的公开理念存档。并非模式或运行时——通过 [stories/ky-cut-surface-generation-vs-consequence.md](../stories/ky-cut-surface-generation-vs-consequence.md) 进行索引。

- **Open-Spek** — [open-spek/loop](https://github.com/open-spek/loop)、[open-spek.github.io](https://open-spek.github.io)（2026年7月）
  一个独立开发、范围不同的“循环工程”术语用法：一种以测试为门槛、全新上下文的循环，将冻结的 Spek——即重建一个软件所需的完整知识——逐步转化为经过验证的实现，而非按计划维护的节奏。首个验证：[open-spek/jsonq](https://github.com/open-spek/jsonq)（21 次迭代、252 个测试、100% 覆盖率、0 行人工编辑代码）。不属于本仓库注册表中的模式——此处交叉链接是因为该术语在两个项目中存在冲突，任一项目的读者都可能混淆两者。

## 生态系统图谱与知识图谱

- **StackMap** — [StackMap 上的循环工程](https://stackmap.shipwithai.xyz/repos/cobusgreyling/loop-engineering)（[交互式图谱](https://stackmap.shipwithai.xyz/?focus=loop-engineering)）（2026年7月）
  开源 AI/智能体工具的精选知识图谱。将 `loop-engineering` 与相关智能体框架和网关（`bemyagent`、`squid`、`atook`、`lynkr`）进行关联映射。关闭了 [#300](https://github.com/cobusgreyling/loop-engineering/issues/300)。

## 本仓库的定位

我们将上述来源视为对该理念当前最佳的阐述，并致力于将抽象框架转化为实用、可复制的模式、模板和工具特定指南（特别关注对原语有强大原生支持的 Grok Build TUI）。

如果您有其他一手来源、真实的生产环境循环案例或修正建议，欢迎提交 Issue 或 Pull Request。