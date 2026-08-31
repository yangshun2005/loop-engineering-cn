# 更新日志

## 0.2.0

- 默认 `--tool` 为 `claude`（原为 `grok`）
- 首周文案和向导默认使用 Claude Code
- 向导痛点 #8：精简循环（GitHub Action，无额外文件）
- 配套增值服务（`--with-foundry` / 记忆 / 舰队）改为可选，不再每次初始化时打印

## 0.1.2

- 在临时 loop-cli 实验后，恢复公开名称 `@cobusgreyling/loop`
- 文档和 `loop` 命令与线上 0.1.0 保持一致

## 0.1.1

- 确认公开包名 `@cobusgreyling/loop`（命令为 `loop`）
- 将 `main` 设置为 `dist/cli.js`
- 文档/示例使用 `@cobusgreyling/loop` 作为入口

## 0.1.0

- 初始统一 CLI 入口：透传 + 诊断/状态/徽章/向导