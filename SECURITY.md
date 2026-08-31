# 安全政策

Loop 工程会对你的代码库运行无人值守的自动化操作。请将 Loop 视为生产环境操作员。

## 报告漏洞

请**私下**报告安全问题——不要为可利用的漏洞创建公开的 Issue。

- **首选方式：** [GitHub 私有漏洞报告](https://github.com/cobusgreyling/loop-engineering/security/advisories/new)
- **电子邮件：** security@cobusgreyling.me（可按需提供 PGP）

有关 Loop 安全的一般性指导，请参阅 [docs/safety.md](docs/safety.md)。

## 无人值守自动化风险

| 风险 | 缓解措施 |
|------|------------|
| 恶意依赖被自动合并 | 黑名单 + 验证器 + 第一周不自动合并 |
| MCP 权限过大 | L1 使用只读连接器；写入权限仅限 PR 评论 |
| 通过提示词窃取机密 | 将 `.env`、凭据路径加入黑名单；切勿在 STATE.md 中记录机密 |
| 无限修复循环消耗预算 | 设置硬性尝试次数上限；在 LOOP.md 中设置终止开关 |
| Loop 生成的 PR 中的供应链风险 | 允许列表之外的任何内容均需人工审查 |

## 进入 L3 前的推荐检查项

- [ ] LOOP.md 中记录了路径黑名单
- [ ] 验证器在隔离的工作树中运行测试
- [ ] 未经明确允许列表，不得自动合并
- [ ] MCP 连接器遵循最小权限原则
- [ ] 存在 `loop-run-log.md` 或等效的可观测性机制

## 支持的版本

| 包 | 支持情况 |
|---------|-----------|
| `@cobusgreyling/loop-audit` | npm 上的最新发布版本 |
| `@cobusgreyling/loop-init` | npm 上的最新发布版本 |
| 参考仓库 `main` 分支 | 当前 HEAD |