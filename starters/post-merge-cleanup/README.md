# 合并后清理 Starter

用于 [合并后清理](../../patterns/post-merge-cleanup.md) 循环（L1 报告 → L2 小修复）的脚手架。

## 快速开始

```bash
# Grok
npx @cobusgreyling/loop-init . --pattern post-merge-cleanup --tool grok

# 或手动复制
cp -r starters/post-merge-cleanup/.grok/skills/post-merge-scan .grok/skills/
cp starters/post-merge-cleanup/post-merge-state.md.example post-merge-state.md
cp starters/post-merge-cleanup/LOOP.md .
```

开始（Grok）：

```
/loop 1d 对合并到 main 的分支（最近 7 天）运行 post-merge-scan。更新 post-merge-state.md。仅在工作树中进行小的文档/链接修复。每次针对同一回归重试前运行 loop-context --check；提交前运行 loop-gate check。升级重构。
```

## 文件

| 文件 | 用途 |
|------|---------|
| `post-merge-state.md.example` | 清理最近合并产生的积压工作 |
| `.grok/skills/post-merge-scan/` | 扫描技能 |
| `LOOP.md` | 节奏和门禁 |

## 安全

- 未经人工工单批准，不得进行大型重构
- 禁止清单：功能开关、认证路径 — 请参阅 [safety.md](../../docs/safety.md)