# Loop 工程操作

一个官方的 GitHub Composite Action，用于在 CI/CD 流水线中安全地执行自主代理。

无需编写复杂的 bash 脚本来实施熔断器、运行就绪审计以及在 worktree 中隔离代理执行，此操作将整个 [Loop 工程](https://github.com/cobusgreyling/loop-engineering) 安全套件封装为一个简洁的步骤。

## 用法

```yaml
- uses: cobusgreyling/loop-engineering/tools/loop-action@main
  with:
    pattern: 'ci-sweeper'
    command: 'npx grok-cli run --skill .grok/skills/ci-sweeper/SKILL.md'
    level: 'L2'
    sandbox: 'true'
    sandbox-shell: 'false'
```

## 输入

| 输入 | 必需 | 默认值 | 描述 |
|-------|----------|---------|-------------|
| `pattern` | **是** | | 您正在运行的循环模式（例如，`ci-sweeper`、`daily-triage`）。必须与 `loop-budget.md` 中的模式匹配。 |
| `command` | **是** | | 调用代理的实际终端命令（例如，`claude`、`grok`、`aider`）。 |
| `level` | 否 | `L1` | 用于预算约束验证的自主级别（`L1`、`L2`、`L3`）。 |
| `sandbox` | 否 | `false` | 如果为 `'true'`，则通过 `loop-sandbox` 路由执行，以提供临时 worktree 隔离，安全地捕获更改而不破坏主工作树。 |
| `sandbox-shell` | 否 | `false` | `loop-sandbox` 的兼容性选项：如果为 `'true'`，则传递 `--shell`，使 `loop-sandbox` 通过其自身的 shell 调用命令。无论此设置如何，`command` 始终作为 Bash 脚本执行（见下文）——`sandbox-shell` 不限制 shell 语法，在正常使用中可以保持默认值。 |

## 底层工作原理

当此操作运行时，它按顺序执行：
1. **`loop-audit`**：根据 Loop 就绪标准检查仓库（确保 `STATE.md`、`gate.yaml` 和约束已就位）。
2. **`loop-context`**：充当熔断器。如果循环陷入失败循环或已耗尽每日令牌预算，则立即停止工作流。
3. **写入命令脚本**：`command` 通过步骤的 `env:` 映射传递（不拼接到 `run:` 脚本文本中），并逐字写入 `$RUNNER_TEMP` 下的临时文件。这可以保持多行命令、引号和 shell 元字符完整，作为单个脚本，而不是被周围的工作流 YAML 重新解析。
4. **执行（`loop-sandbox`）**：如果 `sandbox: 'true'`，该脚本通过 `loop-sandbox` 在临时 git worktree 中作为 `bash -eo pipefail <script>` 调用，该工具将所有修改跟踪到补丁文件中并清理。否则，它直接在主树中执行 `bash -eo pipefail <script>`。

## `command` 的安全使用

`command` 始终通过上述步骤 3 中写入的临时脚本文件作为 Bash 脚本执行——无论 `sandbox` 是 `'true'` 还是 `'false'`，也无论 `sandbox-shell` 如何，都是如此。脚本使用 `bash -eo pipefail` 运行，因此：
- 任何以非零状态退出的命令都会立即停止脚本（`set -e`）
- 管道中任何位置的失败命令都会导致整个管道失败（`pipefail`），而不仅仅是最后阶段

`sandbox-shell` 仅控制 `loop-sandbox` 是否在运行 `bash <script>` 调用之前额外通过其内部 shell（`--shell`）路由该调用——它是一个 `loop-sandbox` 兼容性旋钮，不是您为了在 `command` 中获得 shell 语法（管道、`&&`、多行脚本等）而需要设置的。这些已经通过临时脚本正常工作，与 `sandbox-shell` 无关。

将 `command` 视为工作流中任何其他 `run:` 步骤：

```yaml
- uses: cobusgreyling/loop-engineering/tools/loop-action@main
  with:
    pattern: 'ci-sweeper'
    level: 'L2'
    sandbox: 'true'
    command: |
      npx grok-cli run --skill .grok/skills/ci-sweeper/SKILL.md
      echo "done"
```

支持多行 `command: |` 块，并作为单个原子脚本运行——每一行都在所选路径（沙箱或直接）中执行，而不仅仅是第一行。

> **警告——切勿在 `command` 中直接嵌入不受信任的输入。**
> 不要从 Issue 标题/正文、PR 标题/正文、提交消息、分支名称或任何其他外部不受信任行为者可以影响的内容中插值（例如，`command: 'echo ${{ github.event.issue.title }}'`）。
> `command` 作为任意 shell 脚本，具有作业的权限运行，因此放置在其中的不受信任文本是 shell/命令注入漏洞，无论操作如何在内部传递字符串。
> 如果您需要来自触发事件的数据，请通过中间步骤传递，该步骤验证/转义数据（或将其写入文件，让您的代理读取文件），而不是将其拼接到 `command` 中。