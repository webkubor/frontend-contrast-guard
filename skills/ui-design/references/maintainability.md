# 可维护细则

[ui-design](../SKILL.md) 交付定义的「可维护」部分。目标：下一个 Agent 进来不用猜风格、不用猜怎么跑。

| 项 | 要求 | 工具 / skill |
|---|---|---|
| 设计决策 | 项目根 `DESIGN.md` + tokens；每次方向变更、新增 token 都在「决策记录」记日期、拍板人、放弃项 | [templates/DESIGN.md](../templates/DESIGN.md) |
| Agent 入口 | 项目 `AGENTS.md`（或 CLAUDE.md）写入下方片段 | — |
| README | 安装、本地运行、构建、部署、环境变量说明（不写真实凭据） | — |
| 版本与变更 | CHANGELOG + 版本号；发版时版本号自动同步到文档 | `cs skill show release-sync` |
| 文件规模 | 无超长文件；dev 期实时警告，提交期拦截 | agent-eyes 的 agentSizeWatch / agentGuard |
| 无硬编码 | 组件中无 tokens 之外的颜色/字号/间距/圆角/阴影 | 代码评审 + `rg '#[0-9a-fA-F]{3,8}\b' src` 抽查；agent-eyes `cssVars` 查未声明变量 |
| 视觉质量复查 | 改版后用计量 + 截图找出层级/留白/动效问题 | `cs skill show ui-review` |

## AGENTS.md 片段

```markdown
## UI 改动

- 改 UI 前先读 `DESIGN.md` 与 tokens（`<路径>`），并 `cs skill show ui-design`。
- 只用 tokens；需要新 token 先改 tokens 并在 DESIGN.md 决策记录补一行。
- 不自行更换风格、配色、字体；与需求冲突时问 owner。
```
