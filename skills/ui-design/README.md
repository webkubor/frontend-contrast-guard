# ui-design

UI 设计与前端页面开发的唯一入口。交付物是工程化、可观测、可维护的网站，不只是一张好看的页面。

- **不统一风格**：每个项目有自己的 `DESIGN.md` + tokens；有就照做，没有就先定方向再动手。已有 logo / brand-mark 时整站色彩必须从 logo 推导，不得另起色系。
- **方向阶段就定差异化与 GEO**：竞品扫描、两轴以上差异的严格换名测试（一轴即未通过，禁止假断言）、AI 搜索可引用的内容结构，2–3 个带取舍的候选由 owner 拍板。
- **决策可视化**：需要 owner 拍板的 UI 变更，先在 owner Mac 用 ego-lite（`ego-browser` `taskSpace` + `handOff()`）打开真实预览，同时在 `cs-ui-web` 创建带图决策卡（`decisions.images` 可用前用 `--detail` 图片 URL 链接降级）。
- **质量底线统一**：对比度、键盘与焦点、响应式、遥测接线、提交期 token 守卫。
- **执行层**：方向定好后可调用 [Impeccable](references/impeccable.md) 做 polish / audit / detect，它不是入口。

## 目录

| 路径 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | 流程：找契约 → 存量反推 → 定方向 → 落盘 → 接线 → 验收 |
| [templates/DESIGN.md](templates/DESIGN.md) | 项目设计契约模板 |
| [templates/tokens.css](templates/tokens.css) | tokens 模板 |
| [references/decision-visualization.md](references/decision-visualization.md) | 决策可视化（ego-lite 预览 + 带图决策卡与降级） |
| [references/differentiation.md](references/differentiation.md) | 差异化、logo 导色与严格换名测试 |
| [references/geo.md](references/geo.md) | GEO 内容结构 |
| [references/engineering.md](references/engineering.md) | 工程化接线 |
| [references/observability.md](references/observability.md) | 可观测（agent-eyes 遥测） |
| [references/maintainability.md](references/maintainability.md) | 可维护（tokens 守卫、版本同步） |
| [references/impeccable.md](references/impeccable.md) | Impeccable 执行器接入细则 |

更完整的说明见 [docs/ui-design.md](../../docs/ui-design.md)。
