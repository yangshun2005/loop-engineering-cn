# CI Sweeper 启动模板

用于 [CI Sweeper](../../patterns/ci-sweeper.md) 循环的脚手架。

## 快速开始

1. 复制技能和状态：
   ```bash
   npx @cobusgreyling/loop-init . --pattern ci-sweeper --tool grok
   # 或手动操作：
   mkdir -p .grok/skills
   cp -r starters/ci-sweeper/.grok/skills/ci-triage .grok/skills/
   cp templates/SKILL.md.minimal-fix .grok/skills/minimal-fix/SKILL.md
   cp templates/SKILL.md.verifier .grok/skills/loop-verifier/SKILL.md
   cp starters/ci-sweeper/ci-sweeper-state.md.example ci-sweeper-state.md
   ```

2. 添加 GitHub Action（可选）：`examples/github-actions/ci-sweeper.yml`

3. 开始（Grok）：
   ```bash
   /loop 15m Check CI on main. Update ci-sweeper-state.md. Classify failures. For new actionable failures (not flakes): worktree + minimal-fix + loop-verifier. Run loop-gate check before commit. Escalate after 3 attempts.
   ```

## 不稳定测试策略

如果同一测试在未修改代码的情况下重试时先失败后通过 → 仅观察，不自动修复。