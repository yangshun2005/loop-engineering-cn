# loop-cost

根据执行节奏和就绪级别（L1–L3），估算[循环工程](https://github.com/cobusgreyling/loop-engineering)模式的每日 Token 消耗。

使用 `patterns/registry.yaml` 中的成本元数据。

## 安装与运行

```bash
npx @cobusgreyling/loop-cost --pattern ci-sweeper --cadence 15m --level L2
npx @cobusgreyling/loop-cost --pattern daily-triage --level L1 --json
npx @cobusgreyling/loop-cost --list
npx @cobusgreyling/loop-cost --pattern daily-triage --level L1 --with-caching
```

**从本仓库运行：**

```bash
cd tools/loop-cost
npm install
npm test
```

## 选项

| 标志 | 描述 |
|------|-------------|
| `--pattern` | 模式 ID（参见 `--list`） |
| `--cadence` | 覆盖执行节奏（例如 `15m`、`1d`） |
| `--level` | `L1`、`L2` 或 `L3`（默认 `L1`） |
| `--orchestration` | 多智能体操作成本：`single`（默认）、`maker-checker`、`parallel:N`、`debate:R` |
| `--conservative` | 使用区间内较慢的执行节奏 |
| `--json` | 机器可读输出 |
| `--with-caching` | 添加**缓存**场景：对 `registry.yaml` 中 `stable_fraction` 所定义的稳定（可缓存）部分的报告/操作 Token 应用缓存读取折扣 |

## 场景

每个估算包括：

- **提前退出 / 无操作** — 空监视列表，最小 Token 消耗
- **完整分类** — 每次运行都执行完整扫描
- **每次运行都执行操作** — 每次都执行实现者和验证者（最坏情况）
- **实际混合** — 基于级别的混合（在输出中说明）
- **缓存**（通过 `--with-caching` 选择加入）— 使用 Anthropic 风格的缓存读取定价（约为基础输入的 10%）对 Token 的稳定部分进行折扣；当模式没有 `stable_fraction` 时省略

与 `loop-budget.md`（由 `loop-init` 生成）和 `loop-audit` 成本可观测性检查配合使用。

## 编排成本

上述场景假设只有一次实现者执行。一个分发给多个智能体的循环在**操作路径**上成本更高（无操作扫描和单次分类执行不受影响）。`--orchestration` 应用一个乘数，使得估算值——以及馈送给熔断器的实际单次运行上限——能够反映运行的真实形态：

| 模式 | 乘数 | 模型 |
|------|-----------|--------|
| `single`（默认） | 1x | 一次实现者执行，自检 |
| `maker-checker` | 2x | 实现者 + 独立验证者执行（L2+ 默认） |
| `parallel:N` | N+1 | N 个候选智能体并行分发，外加一次合并/仲裁执行 |
| `debate:R` | 1+R | 一个提议者加上 R 轮批评与修订 |

```bash
npx @cobusgreyling/loop-cost --pattern ci-sweeper --level L2 --orchestration maker-checker
npx @cobusgreyling/loop-cost --pattern ci-sweeper --level L2 --orchestration parallel:3
```

乘数超过 2x 时会发出警告——深度并行分发或辩论模式很容易启用，但在无人值守的情况下运行成本很高。

## 馈送熔断器

[`loop-context`](../loop-context) 的熔断器可以直接从模式的单次运行实际估算中解析 `--token-budget`，而不是手动输入猜测值：

```bash
npx @cobusgreyling/loop-context --check --ledger loop-ledger.json \
  --budget-from-pattern ci-sweeper --budget-level L2
```

`loop-context` 通过调用此 CLI 的内置输出来实现这一点——这两个包在源码层面保持独立，不共享运行时状态。在 `loop-context` 调用中显式指定的 `--token-budget` 始终覆盖派生值。

参见 [docs/operating-loops.md](../../docs/operating-loops.md)。