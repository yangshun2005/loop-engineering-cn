# LOOP.md — Loop 工程参考

本文档说明了 **loop-engineering** 参考仓库如何运用 Loop 工程模式进行运作。

该仓库的目标是成为一套规范、可复制、高信号的模式、入门模板和工具集。它积极践行“自举”（dogfooding）原则。

## 活跃循环

### 每日分类（L1 — 自动化 + 报告）
- 节奏：工作日每天一次（`/.github/workflows/daily-triage.yml`）
- 技能：`loop-triage`（来自 `skills/` 和 `starters/minimal-loop`）
- 状态：STATE.md（由 `scripts/github-triage.mjs` 根据**开放的 PR 和 Issue** 生成，而非 Loop Ready 评分）
- 阶段：仅报告。由人工审查并决定后续行动。
- 交接：被阻塞的 Fork PR、未回复的 Issue、设计决策、新模式的采纳。

### PR 保姆（L2 — 辅助，手动触发）
- 节奏：活跃时段 10–15 分钟（维护者 `/loop` 或未来的 Action）
- 入门模板：`starters/pr-babysitter`（Grok、Claude Code、Codex）
- 使用工作树进行建议修复；需要验证器；默认不自动合并。

### 依赖清理器（L2 — 仅补丁）
- 节奏：6 小时至 1 天
- 入门模板：`starters/dependency-sweeper`
- 前 30 天仅处理补丁和低风险 CVE
- 验证器 = 在工作树中完整执行 `npm ci && npm test`
- 主版本和黑名单包需人工把关

### CI 清理器 / 合并后（机会性）
- `validate-patterns.yml` + `audit.yml` 自举模式验证和就绪度评分
- `audit.yml` 在 PR 上发布循环就绪度评分
- 未来：针对失败的 validate/audit 运行做出反应的清理器

### 变更日志起草器（L1 — 仅草稿，高价值）
- 节奏：每天一次或在发布准备时（手动或标签触发）
- 入门模板：`starters/changelog-drafter`
- 生成 `RELEASE_NOTES_DRAFT.md`（或 GitHub 发布说明的章节）。发布或更新 CHANGELOG 前需人工批准。
- 与合并后清理搭配使用效果极佳，风险低。此参考仓库应在未来发布中运行它。

## 多循环协调

参见 [docs/multi-loop.md](docs/multi-loop.md)。优先级：CI 清理器 → PR 保姆 → 依赖清理器 → 合并后 / 变更日志起草器（非高峰时段）→ 每日分类（报告）。

## 工作树

- 任何无人值守的代码变更实验均在**隔离的 git 工作树**中按尝试运行。
- 每个修复使用一个工作树；验证器拒绝或人工升级后丢弃。

## 连接器（MCP）

- 可选用于 L1 每日分类 — 参见 [examples/mcp/](examples/mcp/)
- GitHub MCP 只读用于 Issue/PR 发现
- 在循环被信任之前，将连接器范围限定为读取 + 评论

## 预算与可观测性

- Token 上限：`loop-budget.md`
- 运行历史：`loop-run-log.md`（由 `daily-triage.yml` 在每个工作日运行后追加）
- 估算：`npx @cobusgreyling/loop-cost --pattern daily-triage`
- 终止开关：`loop-pause-all` 标签或 `STATE.md` 中的标志

## 安全与门禁（本仓库）

- 除琐碎的依赖补丁（白名单 + 验证器）外，主分支不自动合并
- 黑名单：展示用 HTML/CSS、核心原语文档、未经人工审查的审计评分逻辑
- 实时循环状态：仓库根目录下的 `STATE.md`
- `loop-gate check` 根据 `gate.yaml` 机械地强制执行上述黑名单和自动合并白名单；参见 `tools/loop-gate`

## 如何在本地运行

```bash
# 前端入口（构建或 npm 发布后）
node tools/loop/dist/cli.js doctor .
node tools/loop/dist/cli.js status .
node tools/loop-audit/dist/cli.js . --suggest
npx @cobusgreyling/loop init . --pattern daily-triage --tool grok  # npm 发布后
# 等同于：npx @cobusgreyling/loop-init . --pattern daily-triage --tool grok
bash scripts/before-after-demo.sh
```

## 自动化状态（2026-08-28）

| 循环 | 级别 | 自动化 | 备注 |
|------|-------|------------|-------|
| 每日分类 | L1 | ✅ `daily-triage.yml` | 工作日；`github-triage.mjs` 写入 `STATE.md`；`update-last-run-badge.mjs` 写入 `docs/last-run.json` |
| Fork PR 门禁 | L1 | ✅ `fork-pr-gate.yml` | 仅内容的 Fork PR 无需工作流批准即可获得 `validate`/`audit` 状态 |
| 变更日志起草器 | L1 | ✅ `changelog-drafter.yml` | 每周一；创建发布准备 Issue |
| Star 历史 | L1 | ✅ `update-star-history.yml` | 每日；自动 PR 到 `main`；需要 `STAR_HISTORY_TOKEN` 密钥（PAT）。若密钥未设置则跳过（成功）— 对 Fork 安全 |
| 验证 + 审计 | L1 | ✅ `validate-patterns.yml`、`audit.yml` | 在 PR 和推送时；PR 上显示就绪度评分 |
| Dependabot | L1 | ✅ `.github/dependabot.yml` | 每周 npm（根目录、`loop-audit`、`loop-init`、`mcp-server`）+ GitHub Actions |
| PR 保姆 | L2 | ⏸ 手动 | 维护者 `/loop` 或 `starters/pr-babysitter` — 尚无 Action |
| 依赖清理器 | L2 | ⏸ 仅 Dependabot | 通过 Dependabot 提供补丁 PR；完整清理器入门模板为手动 |
| CI 清理器 | L2 | ⏸ 部分 | 通过失败的 validate/audit 做出反应；无专用重试工作流 |

**下一个自动化候选：** 定时运行的 PR 保姆（只读分类评论）、与失败的 `audit.yml` 运行关联的 CI 清理器 workflow_dispatch。

## 演进

历程记录在 `stories/` 中。目标：具备出色可观测性的可靠 L2。

---

*此文件既是文档，也是维护该参考的循环的种子。*