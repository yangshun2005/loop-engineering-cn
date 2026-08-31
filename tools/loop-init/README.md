# loop-init

通过模式和工具将循环工程入门模板搭建到您的项目中。

**`npx @cobusgreyling/loop-init . --pattern daily-triage --tool claude`** 可立即使用。

有关已验证的模式 × 工具矩阵，请参阅 [docs/loop-init-validation.md](../../docs/loop-init-validation.md)。

## 安装与运行

```bash
npx @cobusgreyling/loop-init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop-init . --pattern daily-triage --tool opencode
npx @cobusgreyling/loop-init . -p pr-babysitter -t claude
npx @cobusgreyling/loop-init . -p dependency-sweeper --dry-run
# 一键式 LE → harness-foundry 漏斗：
npx @cobusgreyling/loop-init . --pattern daily-triage --tool claude --with-foundry
```

有关 npm 发布标签，请参阅 [docs/RELEASE.md](../../docs/RELEASE.md)。发布的包中包含来自此 monorepo 的 `starters/` 和 `templates/`。

搭建完成后，始终运行 `npx @cobusgreyling/loop-audit . --suggest`，并实际执行第一个仅报告循环以生成活动信号。

## `--with-foundry`（harness 运行时）

在您的循环文件旁边搭建一个 [harness-foundry](https://github.com/cobusgreyling/harness-foundry) 技术栈：

| LE 模式 | Foundry 预设 |
|------------|----------------|
| `daily-triage`、`issue-triage`、`changelog-drafter` | `minimal` |
| `pr-babysitter`、`ci-sweeper`、`dependency-sweeper`、`post-merge-cleanup` | `implementer` |

创建 `.foundry/stack.yaml`、外层循环钩子存根以及简短的 README。Foundry CLI 上的等效别名：

```bash
npx @cobusgreyling/harness-foundry init --from loop-engineering:daily-triage
```

每次 `loop-init` 运行都会打印一条 Foundry CTA；当 Loop Ready **≥ 80** 时，CTA 会被强调为设计后的下一步。

### 接口提供方（implementer 预设）

`implementer` 预设默认使用内置接口原语。传递 `--model-provider minimax` 以生成 `model/minimax` 提供方原语。生成的技术栈始终包含两个区域端点和两个模型选项，因此您无需重新生成即可切换区域/模型：

```bash
npx @cobusgreyling/loop-init . -p ci-sweeper -t grok --with-foundry --model-provider minimax
npx @cobusgreyling/loop-init . -p ci-sweeper -t grok --with-foundry --model-provider minimax --region cn_zh --model MiniMax-M2.7
```

| 标志 | 值 | 默认值 |
|------|--------|---------|
| `--model-provider` | `anthropic`、`minimax` | `anthropic` |
| `--region` | `global_en`、`cn_zh` | `global_en` |
| `--model` | `MiniMax-M3`、`MiniMax-M2.7` | `MiniMax-M3` |

MiniMax 模型选项和两个端点（`global_en` → `https://api.minimax.io`，`cn_zh` → `https://api.minimaxi.com`）会被写入 `.foundry/stack.yaml`。

## 模式

| 模式 | 默认状态文件 |
|---------|-------------------|
| `daily-triage` | `STATE.md` |
| `pr-babysitter` | `pr-babysitter-state.md` |
| `ci-sweeper` | `ci-sweeper-state.md` |
| `dependency-sweeper` | `dependency-sweeper-state.md` |
| `post-merge-cleanup` | `post-merge-state.md` |
| `changelog-drafter` | `changelog-drafter-state.md` |
| `issue-triage` | `issue-triage-state.md` |

L2 模式（`ci-sweeper`、`dependency-sweeper`）在入门模板中缺少 `minimal-fix` 和 `loop-verifier` 模板时也会复制这些模板。

具备修复能力的模式（`pr-babysitter`、`ci-sweeper`、`dependency-sweeper`、`post-merge-cleanup`）还会获得一个**断路器**：

- `loop-guard` 技能 — 将每次尝试记录到 `loop-ledger.json`，并在重试前运行 [`loop-context`](../loop-context) `--check`
- `loop-ledger.json` — 以模式的目标、其 `pattern`/`level` 以及空的 `attempts` 数组作为种子数据

账本中的 `pattern`/`level` 让 `loop-guard` 能够根据 [`loop-cost`](../loop-cost) 的实际每次运行估算来调整断路器的 `--token-budget`，而不是使用手输的数字。断路器会升级（同一错误连续出现 N 次、连续失败过多、达到令牌预算或迭代上限），而不是徒劳地循环。仅报告模式会跳过此功能。

作用于人工编写、通常规格不明确的输入（`issue-triage`）的模式还会获得一个**intake** 技能：

- `loop-intake` 技能 — 当工作项过于模糊而无法验证“完成”时，它会一次提出一个问题，推动获取精确值，并写入一个开放问题 + `needs-human` 升级，而不是猜测。提前澄清可以让循环不会在从未明确定义的目标上浪费修复尝试。

每个脚手架还会创建：

- `loop-budget.md` — 特定于模式的每日上限和终止开关
- `loop-run-log.md` — 仅追加的运行历史
- `loop-budget` 技能 — 每次运行开始/结束时的运行时预算守护

## 工具

- `grok`（默认）
- `claude`
- `codex`
- `opencode` — daily-triage 附带 `minimal-loop-opencode`（`skills/`、`AGENTS.md`、`opencode.json`）

当某个工具的变体尚不可用时，回退到 Grok 入门路径。

## 从本仓库运行

```bash
cd tools/loop-init && npm ci && npm test
node dist/cli.js /path/to/project --pattern daily-triage --tool grok
```

搭建后与 `loop-audit` 和 `loop-cost` 配合使用：

```bash
npx @cobusgreyling/loop-cost --pattern daily-triage --level L1
npx @cobusgreyling/loop-audit . --suggest
```