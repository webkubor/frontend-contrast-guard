# ui-design

`ui-design` 是 ui-craft 的唯一入口 skill，指导 AI coding agent 读取项目设计规范、设计或还原页面、实现真实组件，并在浏览器截图中发现和修复视觉问题。

- **尊重项目风格**：先读 `DESIGN.md`、tokens、现有页面与产品要求；没有设计契约时，从任务和证据推导，不套通用皮肤。
- **自主推进**：用户授权实现时，记录合理假设后选择方向并继续；只有关键品牌或产品选择无法推断时才请求决策。
- **先规划再实现**：明确页面任务、信息层级、差异化与适用时的 GEO 内容结构；组件的状态和响应式也是设计的一部分。
- **真实页面复查**：固定页面状态与视口，查看截图，按具体元素和根因修复后复查。CLI 色彩检查和样式采样是辅助项，不是审美评分。
- **执行器可选**：方向明确后可调用 [Impeccable](references/impeccable.md) 做 polish / audit / detect；它不是入口。

## 目录

| 路径 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | 完整主流程 |
| [templates/DESIGN.md](templates/DESIGN.md) | 项目设计契约模板 |
| [templates/tokens.css](templates/tokens.css) | CSS tokens 模板 |
| [references/differentiation.md](references/differentiation.md) | 差异化与品牌色参考 |
| [references/geo.md](references/geo.md) | GEO 与页面内容结构 |
| [references/engineering.md](references/engineering.md) | 项目无关的工程接线原则 |
| [references/observability.md](references/observability.md) | 可观测能力的发现与选择 |
| [references/maintainability.md](references/maintainability.md) | DESIGN.md 与 tokens 的维护 |
| [references/impeccable.md](references/impeccable.md) | Impeccable 执行器接入细则 |
| [references/decision-visualization.md](references/decision-visualization.md) | 需要用户决策时的预览与交接参考 |

六阶段总纲见 [docs/ui-design.md](../../docs/ui-design.md)。
