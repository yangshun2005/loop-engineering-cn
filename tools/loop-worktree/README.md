# loop-worktree

为循环工程尝试管理隔离的 [git worktree](https://git-scm.com/docs/git-worktree)。每次修复尝试对应一个 worktree；当验证器拒绝或人工升级时标记它；清理已丢弃的 worktree。

`LOOP.md` 和 `docs/primitives.md` 以文字形式描述了这一约定（“每次修复一个 worktree；在验证器 REJECT 或人工升级后丢弃”）。本工具是该约定的代码实现：为 worktree 提供统一的存放位置、跟踪其状态的清单（manifest），以及清理孤立 worktree 的协调器（reconciler）。

## 安装与运行

```bash
npx @cobusgreyling/loop-worktree create --run-id ci-sweeper-2026-07-07-01 --pattern ci-sweeper
npx @cobusgreyling/loop-worktree list
```

**从本仓库运行：**

```bash
cd tools/loop-worktree
npm install
npm test
```

## 库 API

ESM 使用者可以从包根目录使用 worktree API，并从其受支持的 `lock` 子路径使用锁协调器：

```js
import { createWorktree, cleanupWorktrees } from '@cobusgreyling/loop-worktree';
import { lockPaths, unlockOwner } from '@cobusgreyling/loop-worktree/lock';
```

`lock` 子路径是供需要咨询性路径锁定而无需导入 `dist/` 下私有文件的工具使用的稳定编程入口点。有关 CLI 和 JS 用法，请参阅 [快速入门](../../docs/QUICKSTART.md#l2-isolated-fix-attempts-loop-worktree)。

## 命令

```bash
loop-worktree create --run-id <id> --pattern <p> [--base main]
  # git worktree add -b loop/<id> .loop-worktrees/<id> <base>，并记录清单条目

loop-worktree mark --run-id <id> --status rejected
  # 仅更新清单（审计跟踪）；不删除 worktree

loop-worktree cleanup [--status rejected,escalated] [--older-than 24h] [--force]
  # 对匹配的条目执行 git worktree remove，然后从清单中清除它们

loop-worktree gc [--force] [--json]
  # 将 `git worktree list` 与清单进行对账：
  #   - 磁盘上 .loop-worktrees/ 下存在但不在清单中 -> 报告为孤立项
  #   - 清单中存在但磁盘上不存在 -> 从清单中移除
  # 默认仅报告；--force 删除孤立项

loop-worktree list [--status active] [--json]

loop-worktree lock --paths <glob1,glob2,...> --owner <name> [--ttl 6h] [--wait 15m]
  # 咨询性锁：如果另一个（未过期的）所有者已持有重叠路径，则失败
  # --wait 排队等待而非立即失败；检测等待图死锁

loop-worktree unlock --owner <name>
  # 释放该所有者的锁；如果未持有锁则为空操作

loop-worktree locks [--sweep] [--force] [--json]
  # 列出活动锁和等待意图；--sweep 报告已过期的锁（仅报告；--force 删除它们）
```

## 状态

条目状态为以下之一：`active`、`rejected`、`escalated`、`merged`、`stale`。

`cleanup` 默认清理 `rejected` 和 `escalated` 状态。`active` 状态永远不会被自动清理。

## 安全性

- 如果目录不是 git 仓库，或 `--run-id` 已有活动 worktree，`create` 会以清晰的消息失败（而非原始的 git 错误）。
- `cleanup` 首先执行不带 `--force` 的 `git worktree remove`，因此 git 会拒绝删除含有未提交或未跟踪更改的 worktree；该条目会被报告为已跳过。仅在您接受数据丢失时才传递 `--force`。
- `gc` 默认仅报告，符合仓库中“广泛扫描的操作应报告而非执行”的约定。

## 防止多循环冲突

独立调度的循环可能在同一文件上产生竞争——参见 [stories/multi-loop-collision.md](../../stories/multi-loop-collision.md) 和 [stories/dependency-vs-ci-sweeper-collision.md](../../stories/dependency-vs-ci-sweeper-collision.md) 了解由此引发的真实事件。`lock`/`unlock` 为 [`docs/multi-loop.md`](../../docs/multi-loop.md) 的 `acting_on` 约定提供了机制化形式：

```bash
loop-worktree lock --paths package.json,package-lock.json --owner dependency-sweeper --ttl 6h \
  || exit 2   # 另一个所有者持有重叠路径 -- 跳过本次运行
loop-worktree create --run-id "$RUN_ID" --pattern dependency-sweeper
# ... 执行工作 ...
loop-worktree unlock --owner dependency-sweeper
```

这是刻意设计的咨询性机制，而非由 `create` 本身强制执行——跳过 `lock` 调用的循环不会被物理阻止，符合本仓库现有的“文字约定加工具支持”的理念，而非硬性沙箱隔离。锁定基于路径 glob 而非运行 ID，因此它也能捕获*跨*不同模式（例如 CI Sweeper 和 Dependency Sweeper 同时修改 `package.json`）的冲突，而不仅限于同一模式内部。

`--paths` 的重叠比较按段进行（以 `/` 分割）：通配符段（`*` 或 `**`）与该位置的任何内容兼容，因此 `src/**` 与 `src/foo.ts` 重叠，但 `docs/api` 与 `docs/apidocs.md` 不重叠（不同的字面段，而不仅仅是更短的字符串）。这有意保持简单（咨询性锁，而非完整的 glob 引擎），与本工具中 `--older-than` 解析时长的方式一致。

除非传递 `--ttl`，否则锁不会自行过期；孤立锁（所有者崩溃而未解锁）由 `loop-worktree locks --sweep` 暴露——而非静默忽略——且仅在 `--force` 时删除，与 `gc` 对广泛扫描而非针对单个命名目标的操作默认仅报告的做法一致。`--owner` 仅限于字母、数字、`.`、`_`、`-`（不允许路径分隔符），因为它直接用作锁文件名。并发的 `lock` 调用通过短生命周期的互斥文件进行串行化，因此两个竞争的调用不可能在任一写入之前都通过重叠检查。

## 与 loop-context 配合使用

调用 [`loop-context`](../loop-context) `--check` 并升级的循环控制脚本也应标记其 worktree：

```bash
loop-worktree mark --run-id "$RUN_ID" --status escalated
```

这两个工具保持独立：`loop-worktree` 不读取账本，`loop-context` 不了解 git。

## 清单

`.loop-worktrees/manifest.json`（将 `.loop-worktrees/` 添加到 `.gitignore`）：

```json
{
  "version": 1,
  "worktrees": [
    {
      "id": "ci-sweeper-2026-07-07-01",
      "path": ".loop-worktrees/ci-sweeper-2026-07-07-01",
      "branch": "loop/ci-sweeper-2026-07-07-01",
      "baseBranch": "main",
      "pattern": "ci-sweeper",
      "createdAt": "2026-07-07T08:00:00.000Z",
      "status": "active"
    }
  ]
}
```

## 锁

`.loop-worktrees/locks/<owner>.json`，每个所有者一个文件（同样将 `.loop-worktrees/` 添加到 `.gitignore`——它已涵盖此目录）：

```json
{
  "owner": "dependency-sweeper",
  "paths": ["package.json", "package-lock.json"],
  "lockedAt": "2026-07-14T08:00:00.000Z",
  "expiresAt": "2026-07-14T14:00:00.000Z"
}
```

未指定 `--ttl` 时，`expiresAt` 字段不存在。

参见 [docs/primitives.md](../../docs/primitives.md) 了解 worktree 在“五大构建块 + 记忆”模型中的位置。