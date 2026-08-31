# loop-context

**面向代理循环的有状态内存管理器。** 保持上下文窗口整洁，并在失控循环消耗 Token 之前将其终止。

长期运行的代理循环会以两种经典方式失败，[loop-engineering 文档](../../docs/) 中对此提出了警告：

- **上下文溢出与腐化** — 对话历史和错误日志不断增长，直到模型丢失原始目标或淹没在过时的堆栈跟踪中。
- **停滞/无进展循环** — 代理永远重试相同的失败操作，悄悄耗尽 Token 预算。

`loop-context` 位于代理与其持久内存（`STATE.md`、运行日志）之间，在每次迭代之前，它会执行三项操作：

1. **总结** 已尝试的内容和失败的原因。
2. **修剪** 冗长的堆栈跟踪，合并重复的错误，并丢弃最近窗口之外的过时尝试。
3. **注入** 仅将必要内容注入下一个提示词。

**断路器** 会监控迭代次数、Token 消耗、停滞（相同错误连续出现 N 次）和无进展（连续失败次数过多）——并在徒劳循环之前升级给人工处理。

一切操作都是**确定性的且无依赖**：总结或修剪无需调用 LLM，因此成本低廉，可以在每次迭代中运行。

## 安装 / 运行

```bash
npx @cobusgreyling/loop-context --help
# 或从此仓库运行：
cd tools/loop-context && npm install && npm test
```

## 运行账本

该工具读取**运行账本** — 一个记录循环目标和尝试的 JSON 文件：

```json
{
  "goal": "让失败的迁移测试通过",
  "attempts": [
    { "iteration": 1, "action": "运行迁移", "outcome": "failure",
      "error": "Error: connect ECONNREFUSED 127.0.0.1:5432", "tokensUsed": 1500 },
    { "iteration": 2, "action": "再次运行迁移", "outcome": "failure",
      "error": "Error: connect ECONNREFUSED 127.0.0.1:5432", "tokensUsed": 1400 }
  ]
}
```

`outcome` 为 `success | failure | noop`。`error` 和 `tokensUsed` 为可选字段。

## 用法

```bash
# 断路器 — 退出码 0 = 继续，2 = 升级（接入循环的控制流）
loop-context --check --ledger run.json

# 为下一个提示词压缩上下文块
loop-context --inject --ledger run.json

# 运行的事实汇总
loop-context --summary --ledger run.json --json

# 修剪后的账本（最近窗口、精简跟踪、合并重复项）
loop-context --prune --ledger run.json

# 通过标准输入管道传入
cat run.json | loop-context --check
```

### 选项

| 标志 | 默认值 | 说明 |
|------|---------|---------|
| `--max-iterations <n>` | 10 | 硬性迭代上限 |
| `--stagnation <n>` | 3 | 相同错误连续重复 N 次时升级 |
| `--no-progress <n>` | 5 | 连续 N 次失败后升级 |
| `--token-budget <n>` | 无 | 累计 Token 达到上限时升级 |
| `--window <n>` | 5 | 修剪时保留的尝试次数 |
| `--max-trace-lines <n>` | 8 | 修剪时保留的堆栈跟踪行数 |

退出码：`0` 继续 · `2` 升级 · `1` 错误。

## 从模式解析 Token 预算

手动输入 `--token-budget <n>` 意味着猜测。[`loop-cost`](../loop-cost) 已经为每种模式和就绪级别计算了实际的单次运行估算，因此请从那里解析上限：

```bash
loop-context --check --ledger run.json --budget-from-pattern ci-sweeper --budget-level L2
```

| 标志 | 默认值 | 说明 |
|------|---------|---------|
| `--budget-from-pattern <id>` | 无 | 在 `loop-cost` 的注册表中查找的模式 ID |
| `--budget-level <L1\|L2\|L3>` | `L1` | 传递给 `loop-cost` 的就绪级别 |
| `--budget-scenario <realistic\|action\|report\|caching>` | `realistic` | 用作上限的 `loop-cost` 场景（`caching` 要求模式具有 `stable_fraction`，并向 `loop-cost` 传递 `--with-caching`） |
| `--budget-cadence <spec>` | 模式默认值 | 传递给 `loop-cost` 的节奏覆盖 |
| `--budget-conservative` | 关闭 | 使用范围内的较慢节奏（`loop-cost` 自身的标志） |

显式的 `--token-budget <n>` 始终优先于 `--budget-from-pattern` — 派生值仅在未输入数字时填充。`loop-context` 会调用 `loop-cost` 的内置 CLI（优先使用 monorepo 兄弟包，然后使用已安装的 `@cobusgreyling/loop-cost` 依赖）— 与 `loop-init` 对 `loop-audit` 使用的解析方式相同 — 因此两个包在源码层面保持独立；未知的模式 ID 会显示 `loop-cost` 自身的错误，而不是静默失败。

## 跨独立运行跟踪每日上限

`--budget-from-pattern` 仅保护单次运行的账本。模式还具有**每日**上限（`loop-cost` 的 `suggestedDailyCap`），涵盖一天中的多次运行 — `--daily-budget-from-pattern` 用于跟踪该上限：

```bash
loop-context --check --ledger run.json --daily-budget-from-pattern ci-sweeper \
  --budget-level L2 --on-exceed ./scripts/on-budget-exceed.sh
```

| 标志 | 默认值 | 说明 |
|------|---------|---------|
| `--daily-budget-from-pattern <id>` | 无 | 要跟踪和限制累计每日消耗的模式 ID |
| `--daily-state-dir <dir>` | `.loop-context` | 存储 `daily-spend.<pattern>.json` 的位置 |
| `--on-exceed <script>` | 无 | 在任何升级时，将 `BreakerDecision` 作为 JSON 通过管道传递给此脚本的标准输入 |

每次 `--check` 调用都会将账本最新尝试的 `tokensUsed` 添加到按模式划分的状态文件中（`daily-spend.<pattern-id>.json`，格式为 `{ "date": "...", "tokensUsedToday": N }`），当存储的日期不是今天（UTC）时，计数会归零。一旦累计总数达到模式的建议每日上限，`--check` 会以触发器 `daily-budget` 升级 — 但前提是没有任何单次运行触发器（停滞、无进展、Token 预算、最大迭代次数）已经触发；单次运行的原因始终优先。`--budget-level`、`--budget-cadence` 和 `--budget-conservative`（与 `--budget-from-pattern` 共用）也会影响每日上限的查找。

`--on-exceed <script>` 在**任何**升级时触发，而不仅仅是 `daily-budget` — 它是一个通用钩子，用于自动化 `loop-budget.md` 的“预算超限时”检查清单（暂停调度器、记录事件、通知人工），而不是手动执行这些步骤。脚本的退出码不会被检查，也不会改变 `--check` 自身的退出码。

## 填充账本

您的循环控制脚本会在每次迭代时向 `run.json` 追加一个对象（或通过标准输入传入相同结构）：

```bash
# 每次代理迭代后 — 追加尝试，然后门控下一次
node -e "
const fs = require('fs');
const ledger = JSON.parse(fs.readFileSync('run.json', 'utf8'));
ledger.attempts.push({
  iteration: ledger.attempts.length + 1,
  action: 'run migration tests',
  outcome: 'failure',
  error: process.argv[1],
  tokensUsed: Number(process.argv[2] || 0),
});
fs.writeFileSync('run.json', JSON.stringify(ledger, null, 2));
" \"\$ERROR_MSG\" \"\$TOKENS\"
loop-context --check --ledger run.json || { loop-context --inject --ledger run.json; exit 2; }
```

在循环开始时初始化一次：`{ \"goal\": \"让 main 分支的 CI 通过\", \"attempts\": [] }`。

## 在循环中使用

```bash
# 在循环的控制脚本中，每次迭代之前：
if ! loop-context --check --ledger run.json; then
  loop-context --inject --ledger run.json > escalation.md   # 向人工提供清晰的摘要
  exit 0                                                     # 停止而不是重试
fi
```

## 库 API

```ts
import {
  checkCircuitBreaker,
  pruneLedger,
  summarizeAttempts,
  buildContextInjection,
} from '@cobusgreyling/loop-context';

const decision = checkCircuitBreaker(ledger);
if (decision.escalate) escalateToHuman(decision.reason);
else runNextIteration(buildContextInjection(ledger));
```

## 适用位置

这是循环工程中**内存/状态**原语的动态化实现：`STATE.md` 静态存储状态；`loop-context` 在迭代之间管理状态。参见 [docs/primitives.md](../../docs/primitives.md) 和 [操作与安全](../../docs/) 指南。

MIT · 属于 [loop-engineering](https://github.com/cobusgreyling/loop-engineering)，作者 Cobus Greyling。