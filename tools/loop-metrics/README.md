# loop-metrics

一个基于 `loop-run-log.md` 的可视化 CLI 仪表盘，用于展示自主循环的 Token 消耗、成功率与 ROI。

## 安装

该工具可直接针对你的 loop-engineering 仓库运行：

```bash
npm install -g @cobusgreyling/loop-metrics
```

*（或者，无需安装，直接从项目根目录通过 `npx` 运行即可）*

## 使用方法

在包含 `loop-run-log.md` 的项目根目录下运行仪表盘：

```bash
npx @cobusgreyling/loop-metrics
```

### 过滤

你可以通过特定的循环模式过滤仪表盘，或将时间范围限制在最近 N 天（例如，`7d` 表示最近 7 天）：

```bash
# 仅显示最近 30 天内 daily-triage 模式的指标
npx @cobusgreyling/loop-metrics --pattern daily-triage --timeframe 30d
```

### 选项

- `--log <path>`：循环运行日志的路径（默认：`./loop-run-log.md`）
- `--pattern <id>`：仅计算特定模式的指标。
- `--timeframe <days>`：将计算限制在最近 N 天内的运行（例如，`7d`、`30d`）。
- `--help`：显示帮助信息。

## ROI 的计算方式

`loop-metrics` 使用一个简单的启发式方法来计算 ROI：
- 每次 `action_taken`（成功修复、分类或状态更新）**+10 分**。
- 每次 `escalation`（触发熔断器或需要人工干预）**-5 分**。

这能让你即时、高层地了解你的自主循环是在提供净正向价值，还是仅仅在制造噪音。