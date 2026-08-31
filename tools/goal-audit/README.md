# goal-audit

用于 Grok Build `/goal` 工作流程的 Goal 就绪度评分 CLI。

## 安装 / 运行

```bash
npx @cobusgreyling/goal-audit .
npx @cobusgreyling/goal-audit . --suggest
npx @cobusgreyling/goal-audit . --json
```

## 级别

| 级别 | 分数 | 含义 |
|-------|-------|---------|
| G0 | < 40 | 临时使用 `/goal` |
| G1 | 40–59 | 包含 GOAL.md 及辅助目标 |
| G2 | 60–79 | 包含验证器及测试门禁 |
| G3 | 80+ | 包含 CI、预算、运行日志 |

## 开发

```bash
npm install
npm test
```