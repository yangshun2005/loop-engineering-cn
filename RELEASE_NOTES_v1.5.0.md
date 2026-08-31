# v1.5.0 — 社区工具发布

合并了 7 个社区 Pull Request。新增 3 个工具。一条命令即可体验全部功能。

## 快速开始（复制粘贴）

```bash
# 搭建你的第一个循环（适用于任何 git 仓库）
npx @cobusgreyling/loop-init . --pattern daily-triage --tool grok

# 在调度前检查令牌成本
npx @cobusgreyling/loop-cost --pattern daily-triage --level L1 --cadence 1d

# 评估就绪度并获取后续步骤
npx @cobusgreyling/loop-audit . --suggest

# 检测 STATE.md 与 LOOP.md 之间的配置漂移
npx @cobusgreyling/loop-sync .
```

**初次使用？** [快速入门（5 分钟）](docs/QUICKSTART.md) · [交互式模式选择器](https://cobusgreyling.github.io/loop-engineering/#interactive)

---

## 新增内容

### 新增：`loop-sync` CLI（npm v1.0.0）

检测 `STATE.md`、`LOOP.md` 与技能版本之间的配置漂移。可在 CI 或本地运行：

```bash
npx @cobusgreyling/loop-sync . -v
```

通过 [#47](https://github.com/cobusgreyling/loop-engineering/pull/47) 贡献 — 感谢 @community。

### 新增：`loop-constraints`

结构化约束文件 + 强制执行技能。`loop-init` 现在每次运行都会生成 `loop-constraints.md` 和 `loop-constraints` 技能。`loop-audit` 会对约束的存在性进行评分（当文件和技能同时存在时，就绪度 +6 分）。

通过 [#71](https://github.com/cobusgreyling/loop-engineering/pull/71) 贡献。

### 新增：`loop-mcp-server`（仓库 v1）

用于模式、技能和状态的 MCP 运行时查找。包含路径遍历防护。

```bash
node tools/mcp-server/dist/index.js
```

通过 [#72](https://github.com/cobusgreyling/loop-engineering/pull/72) 贡献。npm 发布即将推出。

### 更新的 npm 包

| 包名 | 版本 | 亮点 |
|---------|---------|------------|
| `@cobusgreyling/loop-audit` | **1.5.0** | 约束评分 + 建议 |
| `@cobusgreyling/loop-init` | **1.2.3** | 约束脚手架；序列化资源打包修复（[#80](https://github.com/cobusgreyling/loop-engineering/pull/80)） |
| `@cobusgreyling/loop-sync` | **1.0.0** | 首个 npm 发布版本 |

### 文档与可发现性

- [QUICKSTART.md](docs/QUICKSTART.md) — 从零到第一个循环的 5 分钟路径
- [OpenClaw 原语矩阵](docs/primitives-matrix.md) — cron、webhooks、心跳映射
- 深色主题 SVG 视觉 + README 优化
- README 中的 Star 历史图表
- daily-triage 和 changelog-drafter 启动器中新增运行后评审部分

---

## 社区

此版本合并了 7 个社区 Pull Request 的工作。在 [Discussion #89](https://github.com/cobusgreyling/loop-engineering/discussions/89) 中阅读完整故事。

**运行了一个循环？** 将你自己添加到 [docs/adopters.md](docs/adopters.md) 或[提交 Add Adopter Issue](https://github.com/cobusgreyling/loop-engineering/issues/new?template=add-adopter.yml)。

**展示与分享：** [讨论 → 展示与分享](https://github.com/cobusgreyling/loop-engineering/discussions/categories/show-and-tell)

---

## 配套 — 目标工程

循环用于发现持续进行的工作。**目标用于完成有界任务。**

| 层级 | 仓库 | 命令 |
|-------|------|---------|
| 发现（节奏） | [loop-engineering](https://github.com/cobusgreyling/loop-engineering) | `npx @cobusgreyling/loop-init . --pattern daily-triage --tool grok` |
| 完成（运行至完成） | [goal-engineering](https://github.com/cobusgreyling/goal-engineering) | `npx @cobusgreyling/goal init . --pattern fix-bug --tool grok` |

**技术栈手册：** [loop → goal → fleet 日常节奏](https://github.com/cobusgreyling/goal-engineering/blob/main/docs/stack-cookbook.md)

```
/goal 读取 STATE.md 中的最高优先级。当验证器通过时完成。goal-verifier 在 completed: true 之前运行。
```