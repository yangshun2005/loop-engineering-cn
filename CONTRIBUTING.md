# 为 Loop Engineering 做贡献

本仓库是一份**实用的工程参考**，而非炒作合集。我们欢迎各种模式、案例、工具映射以及诚实的失败报告。

**维护者响应：** 文档、案例、采用者和小型测试将在 **48 小时内** 获得首次响应（尽可能当天响应）。在 [`good first issue`](https://github.com/cobusgreyling/loop-engineering/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) 上评论 *我来处理* 即可获得任务分配。

## 贡献方式

| 贡献类型 | 位置 |
|--------------|-------|
| 新的循环模式 | `patterns/` + `patterns/registry.yaml` |
| 生产案例 | `stories/` |
| 工具示例 | `examples/{grok,claude-code,codex,openclaw,opencode,github-actions}/` |
| 技能模板 | `templates/` |
| 入门套件 | `starters/` |
| 文档改进 | `docs/` |

## 贡献阶梯

从小处着手——每一个合并的 PR 都很重要。

| 步骤 | 时间 | 贡献内容 |
|------|------|-------------------|
| **1** | 约 15 分钟 | 拼写修复、采用者行、原语矩阵行或 `examples/README.md` 链接 |
| **2** | 约 1 小时 | `stories/` 中的生产案例或 `examples/{tool}/` 中的工具示例 |
| **3** | 半天 | 新的入门套件、技能模板或 MCP 食谱条目 |
| **4** | 一整天 | `patterns/` 中的完整模式 + `patterns/registry.yaml` 条目 |

**最快路径：** [Help wanted (README)](./README.md#help-wanted) · [添加采用者](https://github.com/cobusgreyling/loop-engineering/issues/new?template=add-adopter.yml) · [分享案例](https://github.com/cobusgreyling/loop-engineering/issues/new?template=share-story.yml) · [打开 `good first issue` 待办列表](https://github.com/cobusgreyling/loop-engineering/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)

## 模式要求

每个新模式必须包含 [templates/pattern-template.md](./templates/pattern-template.md) 中的所有章节：

1. 目标（一句话）
2. 调度（每个工具的命令）
3. 所需技能
4. 状态 Schema（含示例）
5. 典型循环（编号步骤）
6. 验证策略（制造者/检查者）
7. 人工交接点
8. 工具特定说明（至少 2 个工具）
9. 失败模式表
10. 成功指标

同时需要在 `patterns/registry.yaml` 中添加条目。

## 案例要求

- 真实经验（如有需要可匿名化）
- 指明所使用的模式
- 至少包含一个失败或意外情况
- 用一段话给出可操作的教训

## 贡献者路径（任选其一）

| 路径 | 适用场景 | 环境准备 |
|------|------|--------|
| **A — 内容** | 案例、示例、采用者、大多数文档 | 无需安装 — 编辑 markdown，提交 PR |
| **B — 单个包** | 修复 `tools/<pkg>` 下的某个 CLI | `cd tools/<pkg> && npm ci && npm test` |
| **C — 完整 monorepo** | 注册表 / 跨包脚本 | 根目录 `npm ci` 然后执行下方命令 |

案例和采用者将尽可能在**当天**获得审查。实时待办列表：[`good first issue`](https://github.com/cobusgreyling/loop-engineering/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)。

## 开发环境设置（路径 C）

```bash
npm ci   # 在仓库根目录 — 安装 yaml/ajv 用于 scripts/validate-registry.mjs
npm run validate:registry
npm run check:loop-init
```

## 领域负责人

参见 [docs/area-owners.md](./docs/area-owners.md)。文档/示例/案例/技能 PR 由 [@AIMindCrafter](https://github.com/AIMindCrafter) **按分配** 进行分诊（非 CODEOWNERS — 自动请求会阻塞 Fork PR）。仅内容的 Fork PR 会从 `.github/workflows/fork-pr-gate.yml` 获得必需的 `validate`/`audit` 状态。

## Pull Request 检查清单

- [ ] 链接在 README 或相关索引中可用
- [ ] 无密钥、令牌或内部 URL
- [ ] `STATE.md` 示例使用 `.example` 后缀（gitignored 实时状态）
- [ ] 安全敏感模式引用 [docs/safety.md](./docs/safety.md)
- [ ] Windows 设置遵循 [docs/QUICKSTART.md § Windows / CRLF 说明](./docs/QUICKSTART.md#windows--crlf-notes)

## 行为准则

本项目遵循 [行为准则](./CODE_OF_CONDUCT.md)（贡献者公约）。

简而言之：

- 工程优先于炒作
- 失败是一等公民内容
- 默认工具无关；工具特定内容放在标注的章节中
- 不容忍骚扰和恶意参与

## 维护者响应（采用者与案例）

仅涉及 `stories/`、`docs/adopters.md` 或 [添加采用者](https://github.com/cobusgreyling/loop-engineering/issues/new?template=add-adopter.yml) 模板的 PR 将尽可能在**当天**获得审查：

1. 维护者在 24 小时内合并或请求一处小修复
2. 在 PR 或 Issue 上公开致谢（`@提及` 贡献者）
3. 如果贡献者希望提交第二个 PR（例如将案例扩展为模式示例），可选择性创建后续 Issue

自动化会在新的案例/采用者 PR 上发布欢迎评论（参见 `.github/workflows/welcome-contributors.yml`）。

## 社区

- **问题**：[随便问](https://github.com/cobusgreyling/loop-engineering/discussions/327)（首选）或使用 `question` 标签提交 Issue
- **展示你的循环**：[展示你的循环](https://github.com/cobusgreyling/loop-engineering/discussions/326)、[添加采用者 Issue](https://github.com/cobusgreyling/loop-engineering/issues/new?template=add-adopter.yml) 或在 [docs/adopters.md](./docs/adopters.md) 中添加一行
- **Loop Ready 徽章**：`npx @cobusgreyling/loop-audit . --badge` — 粘贴到你的 README 中
- **Good first issues**：精选列表见 [README § Help wanted](./README.md#help-wanted)；筛选条件：[`good first issue`](https://github.com/cobusgreyling/loop-engineering/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- **名人堂**：[CONTRIBUTORS.md](./CONTRIBUTORS.md) — 合并后使用 `npm run contributors:generate` 重新生成
- **安全**：参见 [SECURITY.md](./SECURITY.md) — 请勿为可利用的漏洞提交公开 Issue

感谢您帮助我们将此打造为循环工程的权威参考。