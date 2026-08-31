# loop-gate

静态安全策略的机械执行——`docs/safety.md` 中路径拒绝列表和自动合并策略，以及 `LOOP.md` 中 *“除琐碎依赖补丁外，主分支禁止自动合并”* / *“未经人工审查的拒绝列表……”* 规则背后的代码。此前，没有任何机制依据这些文字描述来评估提议的变更；`loop-gate` 正是为此而生。

特意**不感知运行历史**。停滞、重复失败以及令牌/每日预算已属于 [`loop-context`](../loop-context) 断路器 的职责范围——`loop-gate` 仅关注*正在提议的内容*（哪些路径、何种操作类型），而不关心*运行至今的表现*。将两者串联使用：

```bash
loop-context --check --ledger run.json ...            || exit 2   # 基于运行历史
loop-gate check --action auto-merge --paths a.ts,b.ts  || exit 2   # 基于策略
do-the-merge
```

## 安装与运行

```bash
npx @cobusgreyling/loop-gate check --action auto-merge --paths docs/guide.md
```

**从本仓库运行：**

```bash
cd tools/loop-gate
npm install
npm test
```

## 用法

```bash
loop-gate check --action <commit|merge|auto-merge> --paths <f1,f2,...> [--gate-file gate.yaml] [--json]
```

| 标志 | 默认值 | 含义 |
|------|---------|---------|
| `--action <commit\|merge\|auto-merge>` | 必填 | 循环即将执行的操作 |
| `--paths <f1,f2,...>` | 必填 | 逗号分隔的变更文件路径 |
| `--gate-file <path>` | `gate.yaml`（当前工作目录） | 用于评估的策略文件 |
| `--json` | 关闭 | 机器可读的决策输出 |

退出码：`0` 允许 · `2` 升级 · `1` 错误（标志错误、配置缺失或无效）。

## 策略文件

`gate.yaml` 是 `docs/safety.md` 的机器可读对应物——参见 [`templates/gate.yaml.template`](../../templates/gate.yaml.template) 来搭建你自己的策略文件，或使用本仓库自用的 [`gate.yaml`](../../gate.yaml)：

```yaml
version: 1
denylist:
  - ".env"
  - "**/secrets/**"
  - "auth/**"
maxFiles: 10
autoMergeAllowlist:
  - "docs/**"
  - "**/*.md"
```

按顺序检查（首个匹配生效，遵循与 `loop-context` 断路器相同的“最具体触发器优先”约定）：

1. **`denylist`** — 任何匹配此处 glob 模式的变更路径都会升级，无论 `--action` 为何。
2. **`maxFiles`** — 变更路径数超过此值则升级（对应 `docs/safety.md` 中“变更涉及 >N 个文件”的人工门禁）。
3. **`autoMergeAllowlist`** — 仅在 `--action auto-merge` 时检查；每个变更路径必须匹配其中一个 glob 模式，否则升级。

Glob 匹配通过 [`minimatch`](https://www.npmjs.com/package/minimatch) 实现（与 `.gitignore` 风格 glob 相同的语义——`**` 可跨路径段匹配）。

## 本工具不做什么

- 不读取运行账本，也永远不会——这使其在源码层面与 `loop-context` 解耦，正如 `loop-worktree` 和 `loop-context` 保持独立，同时在控制脚本中按约定配对使用。
- 不生成自己的升级摘要格式——将它的 JSON 决策并入你的控制脚本在升级给人工时已从 `loop-context --inject` 组装的内容中。
- 本身不强制执行任何策略——与 `loop-worktree` 的锁类似，这仅是建议性的：跳过调用 `loop-gate` 的控制脚本并不会被物理阻止。该机制的有效性完全取决于调用它的脚本。

参见 [docs/safety.md](../../docs/safety.md) 了解此工具所固化的策略，以及 [docs/primitives.md](../../docs/primitives.md) 了解安全门禁在“五大构建块 + 记忆”模型中的位置。