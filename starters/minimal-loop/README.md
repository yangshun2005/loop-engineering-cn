# 最小循环启动器

将此目录克隆到您的项目根目录中，即可运行一个**仅报告模式的每日分类循环**（L1 就绪状态）。

## 快速开始

1. 将文件复制到您的仓库中：
   ```bash
   cp -r starters/minimal-loop/.grok/skills/loop-triage .grok/skills/  # Grok
   cp starters/minimal-loop/STATE.md.example STATE.md
   ```

2. 自定义 `STATE.md` 中的项目名称。

3. 启动循环（Grok）：
   ```bash
   /loop 1d 运行 loop-triage 技能。先读取 STATE.md。追加高优先级和待观察项。更新最后运行时间戳。第一周内不要自动修复任何内容。
   ```

4. 每天早上阅读 `STATE.md`，持续 1–2 周。调整分类技能。

5. 当分类质量良好时，从 `templates/` 添加 `minimal-fix` 和 `loop-verifier`，并启用小型自动修复功能。

## 包含内容

| 文件 | 用途 |
|------|---------|
| `STATE.md.example` | 状态主干模板 |
| `.grok/skills/loop-triage/SKILL.md` | 分类技能 |
| `LOOP.md` | 面向团队的循环配置文档 |

## 其他工具

- Claude Code: [minimal-loop-claude](../minimal-loop-claude/)
- Codex: [minimal-loop-codex](../minimal-loop-codex/)

## 后续步骤

- [循环设计检查清单](../../docs/loop-design-checklist.md)
- [每日分类模式](../../patterns/daily-triage.md)
- 运行 `npx @cobusgreyling/loop-audit .` 获取就绪评分