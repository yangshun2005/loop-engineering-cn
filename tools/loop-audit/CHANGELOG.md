# 更新日志

`@cobusgreyling/loop-audit` 的所有重要变更均记录在此。

## [1.9.0] - 2026-08-28

### 变更
- 循环活动需要**新的** `Last run`（≤14 天）、带有日期的 `loop-run-log.md` 行，或一个实际触及状态/运行日志文件的 git 提交
- `LOOP.md` 节奏文本和 README 上的 `triage` 提交不再算作活动
- 活动分值为 14 分（原为 6 分）
- 当存在 `.github/workflows/thin-loop.yml` 时，缺少 `STATE.md` 为警告而非失败
- 配套漏斗不再被推送到 `--suggest` 顶部

## [1.8.0] - 2026-07-29

### 新增
- `--auto-fix` 对缺失仓库结构（STATE、LOOP、预算、gate.yaml 等）进行自愈
- 记忆工程就绪信号和 `--with-memory` 建议

## [1.7.0] - 2026-07-20

### 新增
- 针对 LE → harness-foundry 漏斗的 **Harness 运行时** 评分信号：
  - `.foundry/stack.yaml`、`stack.lock`、会话/追踪、外层循环发射路径、主机集成提示
- Loop Ready **80+** 建议/人工报告 CTA → `loop-init --with-foundry`
- `--suggest` 复制 Foundry 脚手架和首次 `foundry run` 的命令

## [1.6.0] - 2026-07-09

### 新增
- 治理评分信号：最小权限工具范围、停滞/无进展检测、人工升级路径
- `SKILL.md` 中的 `allowed-tools` frontmatter 被检测为最小权限信号
- 当治理信号缺失时提供 `--suggest` 建议

## [1.5.3] - 2026-07-06

### 新增
- CLI 运行后的贡献者快速入门 CTA（对 `--json`、`--md` 和 `--badge` 跳过）

## [1.5.2] - 2026-06-30

### 新增
- 检测 `opencode.json` 和 `opencode.json.example` 中的验证代理（Opencode 循环的制造者/检查者分离）
- `--suggest` 复制 Opencode 的命令（`loop-init --tool opencode`）

## [1.5.0] - 2026-06-30

### 新增
- 在就绪评分中检测 `loop-constraints.md` 和 `loop-constraints` 技能（两者同时存在时 +6 分）
- 当约束文件或技能缺失时提供建议

## [1.4.1] - 2026-06-13

### 变更
- 更新了包描述和关键词，以提高在 npm / npx 上的可发现性（强调“循环工程”、编码代理和具体使用示例）。

## [1.3.0] - 2026-06-09

### 新增
- 评分逻辑的单元测试（`test/auditor.test.ts`）
- `--suggest` 现在会提及 `loop-init` 脚手架 CLI
- 审计建议中的镜像仓库和启动器覆盖

### 变更
- CI 在发布前以测试套件为门禁

## [1.2.0] - 2026-06-09

### 新增
- 针对 Grok、Claude Code 和 Codex 的 `--suggest` 从模板复制命令
- 扩展信号：MCP、工作树证据、`patterns/registry.yaml`
- 带有验证器和状态要求的 L3 评分阈值

## [1.1.0] - 2026-06-08

### 新增
- `--md` Markdown 报告格式
- 安全文档和 GitHub 工作流检测

## [1.0.0] - 2026-06-07

### 新增
- 初始 Loop 就绪评分 CLI（L0–L3）
- 用于 CI 集成的 `--json` 输出