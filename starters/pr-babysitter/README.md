# PR Babysitter 启动模板

用于 [PR Babysitter](../../patterns/pr-babysitter.md) 循环（L2 — 带验证器的辅助修复）的脚手架。

## 快速开始

1. 复制到你的仓库中：
   ```bash
   npx @cobusgreyling/loop-init . --pattern pr-babysitter --tool grok
   # 或手动操作：
   cp -r starters/pr-babysitter/.grok/skills/* .grok/skills/
   cp starters/pr-babysitter/pr-babysitter-state.md.example pr-babysitter-state.md
   cp starters/pr-babysitter/LOOP.md .
   ```

2. 根据你的审查规范和必要检查项自定义技能。

3. 开始（Grok）：
   ```bash
   /loop 5m 检查开放的 PR。更新 pr-babysitter-state.md。对于允许列表 PR 上的 CI 失败或可操作的审查评论：工作树 + 最小修复 + 循环验证器。每次重试前运行 loop-context --check；提交前运行 loop-gate check。绝不自动合并——仅提出建议。每个 PR 尝试 3 次后升级处理。
   ```

4. 签署 PR 评论：`🤖 Loop Engineering — PR Babysitter`

## 文件

| 文件 | 用途 |
|------|---------|
| `pr-babysitter-state.md.example` | 监视器状态 |
| `.grok/skills/pr-review-triage/` | PR 分类技能 |
| `LOOP.md` | 团队循环配置 |

## 安全

- 默认不自动合并
- 禁止列表：认证、支付、密钥——参见 [safety.md](../../docs/safety.md)