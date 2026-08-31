# @cobusgreyling/loop

**统一入口**，面向 [Loop Engineering](https://github.com/cobusgreyling/loop-engineering) CLI 工具。

```bash
npx @cobusgreyling/loop init . --pattern daily-triage --tool claude
npx @cobusgreyling/loop doctor .
```

此包**不会**替代 `loop-init`、`loop-audit` 或其他工具。它对这些工具进行封装，并新增了 **doctor** 和 **status** 命令，让第一周的激活流程只需掌握一个心智模型。

| 命令 | 委托给 | 说明 |
|---------|--------------|--------|
| `loop init` | `@cobusgreyling/loop-init` | 脚手架模式 + 工具 |
| `loop audit` | `@cobusgreyling/loop-audit` | Loop Ready 评分 |
| `loop cost` | `@cobusgreyling/loop-cost` | Token 估算 |
| `loop sync` | `@cobusgreyling/loop-sync` | STATE ↔ LOOP 漂移 |
| `loop doctor` | audit + sync + files | **新增** — 前 3 项操作、退出码 |
| `loop status` | files + audit | **新增** — 运行日志仪表盘 |
| `loop badge` | `loop-audit --badge` | README 徽章 |
| `loop context` | `@cobusgreyling/loop-context` | 熔断器 / 记忆 |
| `loop worktree` | `@cobusgreyling/loop-worktree` | 隔离修复尝试 |
| `loop gate` | `@cobusgreyling/loop-gate` | 路径黑名单 |
| `loop mcp` | `@cobusgreyling/loop-mcp-server` | MCP 服务器 |
| `loop sandbox` | `@cobusgreyling/loop-sandbox` | 沙箱辅助工具（如存在） |

## 第一周路径

```bash
# 1. 脚手架（仅报告）
npx @cobusgreyling/loop init . --pattern daily-triage --tool claude

# 2. 一次健康检查
npx @cobusgreyling/loop doctor .

# 3. 可选成本估算
npx @cobusgreyling/loop cost -p daily-triage -l L1 -c 1d

# 4. 第二天仪表盘
npx @cobusgreyling/loop status .
```

### Doctor 退出码

| 代码 | 含义 |
|------|---------|
| `0` | 健康 |
| `1` | 警告（缺口、轻微漂移、预算/运行日志不完整） |
| `2` | 阻塞（评分 &lt; 40、严重漂移或未搭建脚手架） |

```bash
npx @cobusgreyling/loop doctor . --json
npx @cobusgreyling/loop status . --json
```

## 兼容性（旧入口保持可用）

以下命令将永久可用（或直到未来主版本发布并附带长期弃用通知）：

```bash
npx @cobusgreyling/loop-init .
npx @cobusgreyling/loop-audit . --suggest
npx @cobusgreyling/loop-cost --pattern daily-triage
npx @cobusgreyling/loop-sync .
```

`loop <cmd>` 对于透传命令具有相同的二进制行为。

## Monorepo 开发

```bash
# 先构建兄弟工具（doctor 依赖 audit 和 sync）
(cd ../readiness-core && npm ci && npm run build)
(cd ../loop-audit && npm ci && npm run build)
(cd ../loop-sync && npm ci && npm run build)
(cd ../loop-init && npm ci && npm run build)

cd ../loop
npm ci
npm test
node dist/cli.js doctor ../..
node dist/cli.js status ../..
```

## Agent 技能

参见 monorepo 中的 [`skills/install-loop`](../../skills/install-loop/) —— 这是一个简短的技能，Agent 可按照它来安装并检查项目健康状况。

## 许可证

MIT