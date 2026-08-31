# loop-sync

检测并同步仓库中 Loop 配置文件之间的漂移。

## 为什么使用 loop-sync？

在团队协作时，Loop 配置可能会随着时间的推移产生漂移：

- STATE.md 和 LOOP.md 不同步
- 技能已更新但未在配置中体现
- 必需文件缺失
- 配置与初始模板产生偏差

`loop-sync` 能够检测这些问题并提供可操作的建议。

## 安装

```bash
npm install -g @cobusgreyling/loop-sync
# 或
npx @cobusgreyling/loop-sync .
```

## 使用方法

```bash
loop-sync [目标目录] [选项]
```

### 选项

| 选项 | 描述 |
|--------|-------------|
| `-a, --auto-fix` | 使用最小默认值创建缺失的 STATE.md、gate.yaml、loop-budget.md 和 loop-run-log.md，并在 LOOP.md 中添加缺失的 STATE.md 引用。LOOP.md 和 AGENTS.md 绝不会被自动生成——请使用 `loop-init` 生成这些文件。 |
| `-d, --dry-run` | 与 `--auto-fix` 配合使用，仅报告将要创建/更改的内容，不实际写入任何文件 |
| `-v, --verbose` | 显示详细信息 |
| `--json` | 以 JSON 格式输出 |
| `-h, --help` | 显示帮助信息 |

### 示例

```bash
# 基本同步检查
loop-sync .

# 详细输出
loop-sync ./my-project -v

# 用于脚本处理的 JSON 输出
loop-sync ./my-project --json

# 预览 --auto-fix 将要创建/更改的内容，不实际写入任何文件
loop-sync ./my-project --auto-fix --dry-run

# 创建可安全修复的文件并修复 LOOP.md/STATE.md 引用
loop-sync ./my-project --auto-fix
```

## 检查内容

1. **必需文件**
   - STATE.md（必需）
   - LOOP.md（必需）
   - AGENTS.md（推荐）

2. **STATE.md ↔ LOOP.md 一致性**
   - 结构相似性
   - 状态文件引用
   - 模式一致性

3. **技能目录**
   - `.claude/skills/` 是否存在
   - SKILL.md 文件中的版本信息

4. **配置漂移**
   - 缺失引用
   - 孤立文件
   - 不一致之处

## 评分说明

| 分数 | 等级 | 含义 |
|-------|-------|---------|
| 90-100 | 健康 | 未检测到问题 |
| 70-89 | 警告 | 存在轻微不一致 |
| 0-69 | 严重 | 存在需要关注的主要问题 |

## 输出示例

```
Loop 同步报告
══════════════════════════════════════════════════
分数：85/100（健康）

✅ 未检测到问题。配置保持一致。

💡 建议：
   - 运行 loop-init 创建缺失文件
```

## 与 CI/CD 集成

添加到您的 GitHub Actions 工作流中：

```yaml
- name: 运行 loop-sync
  run: npx @cobusgreyling/loop-sync .
```

## 开发

```bash
cd tools/loop-sync
npm install
npm run build
npm test
```

## 许可证

MIT