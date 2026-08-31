# loop-sandbox

为 Loop Engineering 提供临时工作树隔离。将代理执行分流到隔离的 git 工作树中，将其更改捕获为可审查的 `.patch` 文件，并立即销毁沙箱。

## 为什么使用 loop-sandbox？

无人值守的 AI 代理（循环）直接在主工作树中运行可能会造成损害、覆盖未跟踪的文件，或使梳理失败的执行尝试变得极其繁琐。

`loop-sandbox` 通过提供**临时工作树隔离**解决了这一问题：
1. 它会自动为代理创建一个隔离的 git 工作树以供运行。
2. 代理运行其命令，与代码库进行完全自然的交互。
3. 命令完成后，`loop-sandbox` 会自动将代理的编辑（包括新的未跟踪文件）捕获到一个干净的 `.patch` 文件中。
4. 它会销毁工作树，保持主仓库完全干净。

然后，人类可以安全地审查补丁，并按一下按键即可应用。

> [!WARNING]
> **隔离限制：** 这不是容器化的气隙隔离。代理进程保留完整的文件系统和网络访问权限。它可以逃逸工作树（例如 `../../`），并且工作树之外或 `.gitignored` 文件中的更改将**不会**被捕获到补丁中。此工具是对 [docs/safety.md](../../docs/safety.md) 的补充，不能替代针对恶意代码的 OS 级沙箱。

## 安装

```bash
npm install -g @cobusgreyling/loop-sandbox
# 或通过 npx 直接运行
npx @cobusgreyling/loop-sandbox run -- <command>
```

> [!IMPORTANT]
> **必需的 Gitignore：** 您必须将以下内容添加到项目的 `.gitignore` 中，以防止补丁和工作树清单被提交：
> ```
> .loop-sandbox/
> .loop-worktrees/
> ```

## 用法

```bash
loop-sandbox <command> [options]
```

### 命令

| 命令 | 描述 |
|---------|-------------|
| `run [opts] -- <cmd>` | 在隔离沙箱内运行代理命令 |
| `review` / `list` | 列出可供人工审查的隔离补丁 |

### `run` 的选项

| 选项 | 描述 |
|--------|-------------|
| `--shell` | 强制 `shell: true`（用于 `bash -c` 等）。在 Windows 上，通过 npm 安装的 `.cmd` 垫片（`npx`、`tsc` 等）如果因 `ENOENT` 失败，会自动通过 shell 重试，因此在那里很少需要此选项。 |
| `--base <branch>` | 工作树的基础分支（默认为当前 HEAD） |
| `--lock-paths <globs>` | 逗号分隔的 glob 模式，用于在运行期间持有 [`loop-worktree`](../loop-worktree) 咨询锁，以便计划循环无法同时触碰相同的路径。默认关闭——请参阅下面的[多循环安全](#多循环安全)。 |
| `--lock-owner <name>` | 锁所有者名称（默认为运行生成的 id） |
| `--lock-ttl <dur>` | 例如 `30m`——传递给 `loop-worktree` 的 `--ttl` |
| `--lock-wait <dur>` | 例如 `5m`——传递给 `loop-worktree` 的 `--wait` |

### 示例

**通过代理运行工具：**
```bash
# 安全地让代理运行你的 linter/formatter，而不会污染你的工作树
npx @cobusgreyling/loop-sandbox run -- npx my-agent
```

**运行 shell 命令：**
```bash
npx @cobusgreyling/loop-sandbox run --shell -- bash -c "echo 'hello' > test.txt"
```

**与触碰相同文件的计划循环并行运行：**
```bash
npx @cobusgreyling/loop-sandbox run --lock-paths "src/**,docs/**" -- npx my-agent
```

**审查和应用补丁：**
```bash
# 列出沙箱捕获的所有补丁
npx @cobusgreyling/loop-sandbox review

# 示例输出：
# === Loop Sandbox Patches ===
# 📄 sandbox-82f1e394.patch (1.2 KB)
#    Apply: git apply .loop-sandbox/patches/sandbox-82f1e394.patch
```

## 工作原理

在底层，`loop-sandbox` 利用 `@cobusgreyling/loop-worktree` 和原生 `git worktree` 原语。它从当前 HEAD 创建一个临时分支，将你的进程的 `cwd` 设置为隔离树并启动它，运行 `git diff --cached` 生成补丁，并使用 `git worktree remove --force` 进行清理。

## 多循环安全

沙箱运行本身并不能防止计划循环同时编辑相同的文件——`--lock-paths` 使运行选择加入 `loop-worktree` 的咨询锁（在工作树创建之前获取，运行完成后释放），以便冲突的计划循环自身的 `lock` 调用会大声失败，而不是静默竞争。有关此遵循的更广泛约定，请参阅 [docs/multi-loop.md](../../docs/multi-loop.md)。