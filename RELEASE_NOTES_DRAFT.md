# 发布说明草稿 — 2026-07-29 当周（更新于 2026-07-30）

**状态：** 待人工审核的草稿（[#332](https://github.com/cobusgreyling/loop-engineering/issues/332)）。在发布讨论帖或标记软件包之前，请先进行编辑。

**上次发布的讨论：** [讨论 #294](https://github.com/cobusgreyling/loop-engineering/discussions/294)（2026-07-16）— `loop-context` 1.2.0，`loop-worktree` 1.1.0。

**时间窗口：** 2026-07-16 → 2026-07-29

---

## 亮点

### 提示缓存成本模型（已发布）

- **`loop-cost` 1.2.0**（已发布）— `--with-caching` 场景 + 模式上的 `stable_fraction`（[#346](https://github.com/cobusgreyling/loop-engineering/pull/346)，[#347](https://github.com/cobusgreyling/loop-engineering/pull/347)）。感谢 [@Tusm11](https://github.com/Tusm11)。
- **`loop-context` 1.5.0**（已发布）— `--budget-scenario caching` + 挫败感断路器。

### 公共 Worktree 锁 API

- **`loop-worktree` 1.3.1** — 公开的 `./lock` 子路径导出，用于咨询性路径锁定（[#407](https://github.com/cobusgreyling/loop-engineering/pull/407)，感谢 [@shixi-li](https://github.com/shixi-li)），以及并发操作清单丢失修复（[#430](https://github.com/cobusgreyling/loop-engineering/pull/430)）和 npm≥12 打包测试加固（[#421](https://github.com/cobusgreyling/loop-engineering/pull/421)–[#423](https://github.com/cobusgreyling/loop-engineering/pull/423)），后者在烧毁标签之后才落地。
  - 标签：`loop-worktree-v1.3.1`（取代 `loop-worktree-v1.3.0`；参见下方“为何是 1.3.1”）
  - **为何是 1.3.1：** `loop-worktree-v1.3.0` 当前指向 `1c88a5e`（#422），该提交早于 [#423](https://github.com/cobusgreyling/loop-engineering/pull/423) 的打包 tarball 目录扫描修复——而正是这个修复才能在发布工作流的 `npm@latest`（12.x）下存活。因此，2026-07-29 每次触发的 `release-loop-worktree.yml` 都在测试步骤中失败（`package-exports.test.mjs`，因未定义的打包文件名导致 `ERR_INVALID_ARG_TYPE`），未能进入发布阶段；本地复现：`1c88a5e` 树在 npm 12.0.2 下 31/32 失败，当前 `main` 分支 35/35 通过。npm 从未收到 1.3.0。与其重新指向现有标签，不如提升版本号，让发布使用全新标签。

### MiniMax + 记忆桥接（`loop-init` 1.6.0）

- 为 `--with-foundry` 实现者栈提供 `--model-provider minimax`，支持 `--region` / `--model`（[#418](https://github.com/cobusgreyling/loop-engineering/pull/418)）。感谢 [@octo-patch](https://github.com/octo-patch)。
- `--with-memory` 记忆工程桥接脚手架（[#359](https://github.com/cobusgreyling/loop-engineering/pull/359)）。

### 审计自愈（`loop-audit` 1.8.0）

- `--auto-fix` 自愈功能，用于缺失的仓库结构 + 记忆就绪信号（[#358](https://github.com/cobusgreyling/loop-engineering/pull/358)，[#359](https://github.com/cobusgreyling/loop-engineering/pull/359)）。

### MCP 运行时工具（`loop-mcp-server` 1.2.0）

- `loop_audit_score` + `loop_check_breaker` 工具（[#360](https://github.com/cobusgreyling/loop-engineering/pull/360)）。

### 新软件包 — 首次发布

| 软件包 | 版本 | 说明 |
|---------|---------|--------|
| `@cobusgreyling/loop-sandbox` | **1.0.0** | 临时 worktree 隔离 + 咨询性锁（[#370](https://github.com/cobusgreyling/loop-engineering/pull/370)，[#399](https://github.com/cobusgreyling/loop-engineering/pull/399)） |
| `@cobusgreyling/loop-swarm` | **1.0.0** | 基于沙盒的多智能体多数共识（[#398](https://github.com/cobusgreyling/loop-engineering/pull/398)）。感谢 [@THRISHAL12345](https://github.com/THRISHAL12345)。 |

发布顺序：**先 sandbox 后 swarm**（swarm 依赖 `@cobusgreyling/loop-sandbox@^1.0.0`）。

### L3 预算协商器技能

- `budget-negotiator` 技能已集成至 `loop-budget` + 人工安全门控（[#400](https://github.com/cobusgreyling/loop-engineering/pull/400)）。感谢 [@THRISHAL12345](https://github.com/THRISHAL12345)。

### 文档浪潮（Windsurf + QUICKSTART）

| PR | 贡献者 | 主题 |
|----|-------------|--------|
| [#419](https://github.com/cobusgreyling/loop-engineering/pull/419) | @AIMindCrafter | CI Sweeper 生产实践 |
| [#417](https://github.com/cobusgreyling/loop-engineering/pull/417) | @AIMindCrafter | `loop-sandbox` QUICKSTART |
| [#416](https://github.com/cobusgreyling/loop-engineering/pull/416) | @AIMindCrafter | `loop-action` QUICKSTART |
| [#413](https://github.com/cobusgreyling/loop-engineering/pull/413)–[#415](https://github.com/cobusgreyling/loop-engineering/pull/415) | @AIMindCrafter | Windsurf CI / Issue / Dependency 清理器 |
| [#409](https://github.com/cobusgreyling/loop-engineering/pull/409) | @k-anushka14 | QUICKSTART 中的合并门控小节（关闭 #391） |

### 窗口期早期内容（已记录）

- 记忆桥接 + Cursor 示例 + CI 可靠性（[#355](https://github.com/cobusgreyling/loop-engineering/pull/355)–[#362](https://github.com/cobusgreyling/loop-engineering/pull/362)）
- 原语矩阵：Continue.dev、Copilot、Cline、Roo Code

---

## 软件包状态（截至 2026-07-30，2026-07-29 发布尝试之后）

| 软件包 | npm 上版本 | 目标版本 | 状态 | 操作 |
|---------|--------|--------|-------|--------|
| `@cobusgreyling/loop-cost` | **1.2.0** | 1.2.0 | 完成 | — |
| `@cobusgreyling/loop-context` | **1.5.0** | 1.5.0 | 完成 | — |
| `@cobusgreyling/loop-worktree` | **1.2.0** | **1.3.1** | `v1.3.0` 标签已烧毁（其提交树早于 #423 的打包测试修复） | 本 PR 将版本提升至 1.3.1 → 标签 `loop-worktree-v1.3.1` |
| `@cobusgreyling/loop-init` | **1.5.0** | **1.6.0** | 标签已存在；07-29 运行在构建阶段通过，发布阶段失败（E404） | 按下方仓库配置，然后重新触发 |
| `@cobusgreyling/loop-audit` | **1.7.0** | **1.8.0** | 标签已存在；测试通过，发布 E404 | 按下方仓库配置，然后重新触发 |
| `@cobusgreyling/loop-mcp-server` | **1.1.0** | **1.2.0** | 标签已存在；测试通过，发布 E404 | 按下方仓库配置，然后重新触发 |
| `@cobusgreyling/loop-sandbox` | — | **1.0.0** | 标签已存在；测试通过，发布 E404（首次发布） | 参见下方首次发布说明 |
| `@cobusgreyling/loop-swarm` | — | **1.0.0** | 等待 sandbox | 在 sandbox 发布到 npm 后进行首次发布 |
| `@cobusgreyling/loop-gate` | **1.0.0** | 1.0.0 | 完成 | — |
| `@cobusgreyling/loop` | **0.1.2** | 0.1.2 | 完成 | — |

### 为何每次 07-29 发布都以 `npm error 404`（PUT）失败

所有三轮发布均以 `404 Not Found - PUT https://registry.npmjs.org/@cobusgreyling%2f<pkg>` 结束，包括在 #422 从发布步骤中移除 `NODE_AUTH_TOKEN` **之后**运行的 15:10 那一轮。在每份日志中，Sigstore 来源声明都在 PUT 失败**之前**完成签名和上传，因此 GitHub OIDC 令牌本身是有效的——是仓库在拒绝**未认证**的发布（npm 对未授权的软件包访问报告 404 而非 403）。这指向 npmjs.com 端的可信发布配置，而非工作流本身：

- 可信发布在 npmjs.com 上按**软件包**进行配置（软件包 → 设置 → 可信发布者），且**工作流文件名必须完全匹配**。本仓库为每个软件包使用一个工作流（`release-loop-worktree.yml`、`release-loop-audit.yml`、`release-loop-init.yml`、`release-loop-mcp-server.yml`、`release-loop-sandbox.yml`，……），因此每个软件包都需要自己的条目，指向其各自的工作流文件名。
- **首次发布**（`loop-sandbox`、`loop-swarm`）：可信发布者只能配置在已存在于仓库中的软件包上。如果 npmjs.com 不提供针对该 scope 的“使用可信发布者创建软件包”流程，则需要从维护者机器上使用细粒度令牌手动执行一次首次发布（`npm publish --access public`），然后再添加可信发布者，让 CI 接管后续发布。

### 建议的发布顺序（人工门控）

1. 合并本 PR（将 `loop-worktree` 提升至 1.3.1；其他软件包的标签已存在，且其标签提交树测试通过）。
2. 在 npmjs.com 上，为上述每个软件包添加可信发布者条目（仓库 `cobusgreyling/loop-engineering`，使用软件包自己的 `release-*.yml` 文件名，环境留空除非已配置）。
3. 标记取代版本的 worktree 发布：
   ```bash
   git tag loop-worktree-v1.3.1 && git push origin loop-worktree-v1.3.1
   ```
   （保留 `loop-worktree-v1.3.0` 不变；npm 从未收到 1.3.0，发布触发不得以它为目标。）
4. 针对现有标签重新触发失败的工作流：`loop-audit-v1.8.0`、`loop-init-v1.6.0`、`loop-mcp-server-v1.2.0`、`loop-sandbox-v1.0.0`（或按上方说明手动首次发布 sandbox）。一旦 sandbox 发布到 npm，创建 swarm 标签（该标签尚不存在）：
   ```bash
   git tag loop-swarm-v1.0.0 && git push origin loop-swarm-v1.0.0
   ```
5. 使用 `npm view @cobusgreyling/<pkg> version` 确认。
6. 将本草稿整理为 GitHub Discussion；关闭 [#332](https://github.com/cobusgreyling/loop-engineering/issues/332)。

---

## 试用（发布后）

```bash
npx @cobusgreyling/loop-worktree --help
npx @cobusgreyling/loop-init . --pattern daily-triage --tool grok --with-foundry --model-provider minimax
npx @cobusgreyling/loop-audit . --auto-fix
npx @cobusgreyling/loop-sandbox --help
npx @cobusgreyling/loop-swarm run --count 3 -- echo "demo"
```

---

## 日常维护

- PR 分类 2026-07-29：合并了文档浪潮 + #398/#400/#407/#418；#409 为增量返工；#365 发布草稿刷新已被本文件取代。
- 功能 PR 应在同一变更中包含 `package.json` 版本提升（教训：未发布的功能以旧版本停留在 main 分支上）。