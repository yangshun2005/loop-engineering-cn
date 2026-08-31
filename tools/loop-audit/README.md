# loop-audit

用于评估项目 **Loop 就绪度**（0–100 分）并建议后续步骤的 CLI 工具。

**npx @cobusgreyling/loop-audit . --suggest** 可立即使用（已发布包）。

## 安装与运行

**npm（推荐）：**

```bash
npx @cobusgreyling/loop-audit .
npx @cobusgreyling/loop-audit . --suggest
```

**从本仓库运行：**

```bash
# readiness-core 是 monorepo 同级目录（file: 依赖）—— prebuild 会为你构建它
cd tools/loop-audit
npm install
npm run build   # 运行 prebuild → 构建 ../readiness-core，然后执行 tsc
node dist/cli.js /path/to/your/project
```

## 前后对比演示

查看分数从空 → L1 入门 → L2 验证者的攀升过程：

```bash
bash scripts/before-after-demo.sh
```

## 选项

```bash
loop-audit .              # 人类可读（默认）
loop-audit . --json       # 机器可读
loop-audit . --md         # Markdown 报告
loop-audit . --suggest    # 从模板复制命令 + 活动建议（所有工具）
loop-audit . --badge      # Markdown README 徽章（Loop 就绪级别 + 分数）
```

如果分数 < 40，退出代码为 `2`（一旦项目达到 loop-ready，可用于 CI 门禁）。

## 发布到 npm

维护者：

```bash
cd tools/loop-audit
npm run build
npm publish --access public
```

## 检查的信号（v1.7+）

| 信号 | 说明 |
|-------------------------|-------|
| 状态文件 | STATE.md 或特定模式 |
| 分诊技能 | loop-triage / ci-triage / pr-review-triage 等 |
| 验证者技能 | 制造者/检查者分离（技能或 Claude/Codex 代理） |
| LOOP.md / 配置 | 节奏、限制、交接 |
| AGENTS.md / CLAUDE.md | 项目约定 |
| 安全文档 | safety.md + LOOP.md 中提及的门禁 |
| .github/ + workflows | 自举 / 自动化 |
| MCP / 连接器 | 提及或配置文件 |
| Worktree 证据 | 文档中的隔离模式 |
| patterns/registry.yaml | 工具化的机器索引 |
| loop-budget.md | Token 上限和终止开关 |
| loop-run-log.md | 仅追加的运行历史 |
| LOOP.md 预算部分 | 配置中记录的节奏限制 |
| loop-budget 技能 | 运行时预算保护 |
| 最小权限工具范围 | SKILL.md 中的 `allowed-tools`，或文档化的工具/MCP 范围（代理仅获得其角色所需的权限） |
| 停滞 / 无进展检测 | loop-context 熔断器、台账，或文档化的最大尝试次数 / 无进展规则 |
| 人工升级路径 | LOOP.md / 安全文档定义何时停止并移交给人工 |
| **loopActivity（v1.4）** | **动态证明**：状态中的“上次运行”时间戳、与 loop 相关的 git 提交、定时工作流、运行日志 |
| **Harness 运行时（v1.7）** | `.foundry/stack.yaml`、锁、会话/追踪、外层循环发射、主机集成 — LE → [harness-foundry](https://github.com/cobusgreyling/harness-foundry) 漏斗 |

当分数 ≥ 80 且不存在 `.foundry/stack.yaml` 时，审计建议：

```bash
npx @cobusgreyling/loop-init . --with-foundry
```

L3 需要验证者 + 状态 + 成本可观测性（预算 + 运行日志 + LOOP.md 预算）**以及**经过验证的 loop 活动（不仅仅是磁盘上的文件）。

## 级别

| 级别 | 含义 |
|-------|---------|
| L0 | 草案 — 记录意图 |
| L1 | 仅报告型 loops |
| L2 | 带验证者的辅助自动修复 |
| L3 | 可无人值守（带人工门禁） |

参见 [docs/loop-design-checklist.md](../../docs/loop-design-checklist.md)。