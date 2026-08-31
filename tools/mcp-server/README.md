# @cobusgreyling/loop-mcp-server

面向 **loop-engineering** 的 MCP（模型上下文协议）服务器——将模式、技能、状态、预算和审计工具作为可运行时查询的资源暴露给 AI 代理。

无需将所有循环文档塞入提示词中，代理可以通过 MCP 按需仅查询所需内容。

## 快速开始

```bash
npx @cobusgreyling/loop-mcp-server
```

从克隆的 `loop-engineering` 仓库中：

```bash
cd tools/mcp-server && npm ci && npm test
node dist/index.js
```

将 `LOOP_PROJECT_ROOT` 设置为你目标项目（默认为 `cwd`）。

### 在 Claude Code / Grok / 任何 MCP 客户端中配置

添加到你的 MCP 配置中（`.mcp.json` 或等效文件）：

```json
{
  "mcpServers": {
    "loop-engineering": {
      "command": "npx",
      "args": ["-y", "@cobusgreyling/loop-mcp-server"],
      "env": {
        "LOOP_PROJECT_ROOT": "."
      }
    }
  }
}
```

## 资源

| URI | 描述 |
|-----|------|
| `loop://registry` | 模式注册表（全部 7 种模式，包含元数据、成本、阶段） |
| `loop://config` | LOOP.md — 节奏、预算、门禁、调度 |
| `loop://budget` | loop-budget.md — Token 上限、紧急停止开关 |
| `loop://run-log` | loop-run-log.md — 仅追加的运行历史 |
| `loop://safety` | 安全文档 — 黑名单、自动合并策略、MCP 作用域 |
| `loop://patterns/{id}` | 按 ID 查看完整模式文档 |
| `loop://skills/{name}` | 按名称查看技能定义（SKILL.md） |
| `loop://state/{file}` | 状态文件内容 |

## 工具

| 工具 | 描述 |
|------|------|
| `loop_list_patterns` | 列出所有模式及其目标、节奏、风险等级 |
| `loop_list_skills` | 列出可用技能及其位置 |
| `loop_list_state_files` | 列出项目中的状态文件 |
| `loop_get_pattern` | 获取完整模式文档 + 注册表元数据 |
| `loop_get_skill` | 获取指定技能的 SKILL.md 内容 |
| `loop_get_state` | 读取状态文件以获取当前循环状态 |
| `loop_recommend_pattern` | 根据用例描述推荐模式 |
| `loop_estimate_cost` | 估算模式在 L1/L2/L3 级别的每日 Token 成本 |

## 环境变量

| 变量 | 默认值 | 描述 |
|------|--------|------|
| `LOOP_PROJECT_ROOT` | `cwd()` | 要服务的项目根目录 |

## 开发

```bash
cd tools/mcp-server
npm install
npm run build
npm test
```

## 架构

```
Agent (Claude Code / Grok / Codex)
  │
  ├─ MCP 资源读取 ──→ loop://patterns/daily-triage
  ├─ MCP 工具调用 ──→ loop_recommend_pattern("watch CI failures")
  └─ MCP 工具调用 ──→ loop_estimate_cost("ci-sweeper", "L2")
  │
  ▼
loop-mcp-server（stdio 传输）
  │
  ├─ resolver.ts ──→ 读取 patterns/、skills/、STATE.md、LOOP.md 等
  └─ index.ts ──→ MCP 协议处理器
```

服务器从 `LOOP_PROJECT_ROOT` 处的本地文件系统读取数据。它是只读的——永远不会写入项目。

## 另请参阅

- [Loop 工程模式](../../patterns/)
- [MCP 示例](../../examples/mcp/)
- [原语：插件与连接器](../../docs/primitives.md)
- [安全：MCP 最小权限](../../docs/safety.md)