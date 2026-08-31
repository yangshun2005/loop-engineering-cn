# 更新日志

`@cobusgreyling/loop-init` 的所有重要变更均记录在此。

## [未发布]

## [1.7.0] - 2026-08-28

### 新增
- `thin-loop` 模式：单个 GitHub Action，无需 `STATE.md` / 技能 / 预算文件
- 默认 `--tool` 为 `claude`

### 变更
- 配套 CTA（`--with-foundry`、`--with-memory`、`--with-fleet`）仅在传递相应标志时打印

## [1.6.0] - 2026-07-29

### 新增
- 为 `--with-foundry` 实现者技术栈新增 `--model-provider minimax` — 生成 `model/minimax` 提供程序原语，而非默认的接口原语
  - 每个生成的技术栈均包含全球（`global_en`）和中国（`cn_zh`）端点配置
  - `MiniMax-M3`（默认）和 `MiniMax-M2.7` 模型选项，包含上下文窗口、输入模态、思考模式和定价
- 新增 `--region`（`global_en`、`cn_zh`）和 `--model`（`MiniMax-M3`、`MiniMax-M2.7`）标志，用于选择活跃的 MiniMax 区域和模型
- 新增标志的帮助文本和示例
- 新增 `--with-memory` 记忆工程桥接脚手架（层级 + 预算信号）

## [1.5.0] - 2026-07-20

### 新增
- `--with-foundry` — 一键式 LE → harness-foundry 漏斗
  - 生成 `.foundry/stack.yaml`、钩子、会话目录和 README
  - 模式映射：仅报告 → `minimal`，可修复 → `implementer`
- 每次运行后的脚手架后 Foundry CTA（当 Loop Ready ≥ 80 时更强）
- Foundry 预设的帮助文本和示例

## [1.4.0]

- 先前版本（启动器、断路器、入口、可观测性脚手架）