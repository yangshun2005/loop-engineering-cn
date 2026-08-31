# AGENTS.md — loop-engineering 参考

维护此仓库的人类与循环（loops）的约定。

## 构建与验证

```bash
# 统一入口（新工作流推荐使用）
cd tools/loop && npm ci && npm test
node dist/cli.js doctor ../..
node dist/cli.js status ../..

# Loop 就绪度审计 CLI（仍是规范评分器）
cd tools/loop-audit && npm ci && npm run build
node dist/cli.js ../..              # 审计仓库根目录
node dist/cli.js ../.. --suggest    # 显示缺失项的复制命令

# 前后对比演示（对空目录 → 起始模板 → L2 进行评分）
bash scripts/before-after-demo.sh
```

入口映射：[docs/cli-front-door.md](docs/cli-front-door.md)。Agent 安装技能：[skills/install-loop/SKILL.md](skills/install-loop/SKILL.md)。

CI 在每次推送/PR 时运行 `validate-patterns` 和 `audit`（参见 `.github/workflows/`）。

## 审查规范

- 模式（Patterns）和起始模板（Starters）必须保持**意图上的工具无关性**；工具特定路径位于 `examples/` 和各个工具的起始模板下。
- 未经人工审查，不得自动合并对 `docs/primitives*.md`、`tools/loop-audit/src/` 或展示资源的更改。
- `stories/` 中的失败案例应包含 token 成本、根本原因和补救措施——而不仅仅是成功案例。
- 新模式需要在 `patterns/registry.yaml` 中添加条目。

## 循环操作（本仓库）

- **每日分类**：`loop-triage` 技能 → `STATE.md`（仅报告，L1）。
- **修复**：仅通过带人工审查的 PR 进行；`minimal-fix` + `loop-verifier` 用于辅助更改（L2）。
- **隔离**：对于任何无人值守的代码更改实验，使用 git worktrees（参见 `LOOP.md`）。

## 测试命令

本仓库没有应用程序测试套件。质量门禁：

```bash
cd tools/loop && npm test && node dist/cli.js doctor ../..
cd tools/loop-audit && npm run build && node dist/cli.js ../../
bash scripts/before-after-demo.sh
```