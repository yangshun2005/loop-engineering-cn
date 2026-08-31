# 依赖清理循环

**目标**：发现过时或有漏洞的依赖，应用最小的安全更新，在隔离环境中通过测试/构建进行验证，并将任何有风险的操作（重大版本升级、破坏性变更或高危漏洞）上报给人工处理。

## 调度

**推荐方案**：
- Grok：`/loop 6h dependency-sweeper`（或工作时间内每日运行）
- Claude Code：`/loop 12h /sweep-deps` + `/goal keep the dependency surface clean and green`
- Codex：计划自动化任务，每 4-12 小时运行一次分类 + 最小修复流程
- GitHub Actions：Dependabot + 定时或 workflow_dispatch 触发的清理器（参见 `examples/github-actions/` 模式）

在 Dependabot 或 OSV 警报触发后应提高运行频率。在重大版本发布周期中应降低频率或暂停。

## 所需技能

- `dependency-triage` — 解析锁文件/包清单，按风险对更新进行分组（补丁/次要/主要 + 已知 CVE），生成简短的可操作列表，包含建议的下一版本和理由。
- `minimal-fix` — 应用尽可能小的变更（通常仅版本升级 + 锁文件更新），不做其他任何修改。
- `loop-verifier`（或项目测试脚本）— 在干净的工作树中运行项目的测试/构建/代码检查，确认变更未引入回归问题。

## 状态

文件名：`dependency-sweeper-state.md`

保持进行中更新、上次操作和人工决策的紧凑列表：

```markdown
- package.json (lodash): 4.17.21 → 4.17.21 (CVE-2021-23337) — 已应用补丁，测试通过
  上次操作：2025-06-09T09:12Z
  人工决策：批准 express 的次要版本升级（本迭代中该包在禁止列表中）
- (已解决) react: 次要版本升级 — 已合并
```

每次运行时清理已合并/已关闭的条目。

## 循环运行方式（典型周期）

1. 扫描清单 + 锁文件 + 所有安全公告（Dependabot、OSV、npm audit 等）。
2. 分类为安全（补丁 + 无 CVE）、谨慎（次要）和高风险（主要或高危 CVE）。
3. 对安全项：创建 worktree，通过 minimal-fix 技能应用最小版本 + 锁文件更新，运行验证器（测试 + 构建）。
4. 如果验证器通过 → 创建或更新一个小型 PR（或在允许的情况下提交到依赖分支）并记录在状态中。
5. 对谨慎/高风险项 → 添加到状态中，标注明确的“需要人工处理”标志 + 上下文（变更日志差异摘要、CVE 链接）。
6. 更新状态文件，（可选）在开放的依赖 PR 上评论。
7. 下次运行时，重新评估开放项并关闭过期或冲突的项。

## 熔断器

这是一个可修复（L2）模式，因此 `loop-init` 会自动搭建 `loop-guard` 技能并为其生成种子化的 `loop-ledger.json`。在每次对同一包重试之前，运行熔断器：

```bash
npx @cobusgreyling/loop-context --check --ledger loop-ledger.json \
  --budget-from-pattern dependency-sweeper --budget-level L2
```

如果相同失败重复出现或达到尝试上限（参见下方“重试 2 次后升级”），它将非零退出 — 停止重试并升级给人工处理，而不是继续循环。参见 [docs/safety.md](../docs/safety.md)。

## 验证策略

- **绝不**让实现者子代理声明成功。
- 始终在隔离的 worktree 中运行验证者子代理（或显式的 `npm test && npm run build`）。
- 循环只提出变更或 PR。实际合并由人工（或“已知安全补丁”的允许列表）执行。
- 对于安全更新，优先选择解决公告的最小补丁。

## 人工交接点

- 任何主要版本升级或一次性影响超过 N 个包的锁文件变更。
- 高危或严重 CVE（即使已有补丁）。
- 明确禁止列表中的包（认证、支付、核心基础设施、大型内部 monorepo 包）。
- 同一依赖在 N 次自动尝试后仍无进展。
- 验证器 worktree 中测试或类型检查失败。

## 工具特定说明

**Grok Build TUI**：
- 使用 `/loop 6h Run dependency-triage. For safe patches create minimal PRs in worktrees. Never auto-merge majors or CVEs above medium.`
- 与调度器和 worktree 原语结合使用。

**Claude Code**：
- Boris 风格循环在此场景下效果极佳：一个循环用于发现/分类，子代理用于最小版本升级 + 验证。
- 使用 `/goal` 保持“依赖面干净，无高危漏洞且少于 5 个开放的次要更新”。

**GitHub Actions / Dependabot**：
- 让 Dependabot 创建 PR。清理器的工作变为“对 Dependabot PR 进行分类，如果机器人的 PR 被阻塞则应用最小额外修复，变基，并对有风险的 PR 提醒人工审查”。
- 参见 `examples/github-actions/` 中的示例工作流。

**Codex**：
- 调度一个周期性 Codex 任务，对 package.json + package-lock.json 运行分类技能，生成状态更新 + 建议差异。

## 失败模式与缓解措施

| 失败 | 缓解措施 |
|---------|------------|
| 循环应用了“安全”更新但在生产环境中破坏构建 | 在 worktree 中使用强验证器 + 未经明确允许列表绝不自动合并任何内容 + 超出补丁级别的任何变更均需人工审查。 |
| 无限更新循环（A 依赖 B，B 又回退） | 限制每包每 24 小时的尝试次数。在状态中记录上次尝试的版本。重试 2 次后升级。 |
| 安全补丁在其他地方引入新漏洞 | 验证器应在变更后包含 `npm audit`（或等效工具）。 |
| 更新一次性影响数十个传递依赖 | 视为高风险。在 minimal-fix 步骤中仅处理直接依赖。 |
| 通知轰炸 | 仅对状态中“需要人工处理”部分的项目通知人工。其他一切静默或每日汇总。 |

## 成本概况

| 场景 | 每次运行 Token | 备注 |
|----------|------------|-------|
| 无操作（无需升级） | ~5k | 扫描干净时退出 |
| 分类 / 扫描 | ~60k | 审计 + Dependabot + 锁文件扫描 |
| 补丁 + 验证（L2） | ~300k | Worktree + 完整测试套件 |

**节奏**：6h–1d · **层级**：中等 · **建议每日上限**：500k tokens

```bash
npx @cobusgreyling/loop-cost --pattern dependency-sweeper --level L2
```

验证器运行（`npm ci && npm test`）占成本主导 — 在 `loop-budget.md` 中限制每包的尝试次数。

## 成功指标

- 从“漏洞发布 / Dependabot PR 创建”到合并的中位时间（针对循环处理的项目）。
- 开放超过 48 小时的高危/严重漏洞数量（目标接近零）。
- 纯粹是“LGTM，循环处理了版本升级 + 验证”的依赖 PR 百分比。
- Slack/Linear 中“能升级一下 X 吗？”消息的减少。

从**仅补丁级别 + 已知 CVE 修复**开始，在单个仓库上运行 1-2 周。只有在信任验证器后才扩展到次要版本。主要版本和破坏性变更应在很长时间内保持人工把关。