---
name: ui-design
version: 2.1.0
description: "帮助 AI coding agent 做出更好的 UI：从读取项目设计规范、确定页面层级与风格，到实现组件、检查真实截图、修复视觉问题。遵循已有品牌和 tokens；无契约时在授权范围内记录假设、建立 DESIGN.md 并继续实现；完成后按项目规范验证响应式、可访问性与交互。适用于新页面、存量改版、组件设计、UI 评审与视觉修复。"
license: MIT
---

# UI Design

## 总纲

这是 UI skill：帮助 Agent 按项目设计契约做出更好的组件和页面，并检查真实浏览器效果。细节由本 skill 按用户授权范围处理。

- **跨项目风格自由**：不同项目应该长得不一样，本 skill 不带默认皮肤。
- **项目内风格一致**：风格真源是**项目自己的 DESIGN.md + tokens**，不是 Agent 当下的审美。改 UI 前先找它，没有就先定方向，不准自己编风格。
- **质量底线统一**：层级、间距、对比度、交互状态、响应式、GEO、工程接线，所有项目同一条线。

**触发优先级**：做 UI 时**先加载本 skill**（`ui-design` 是唯一入口）。`impeccable` 是本 skill 按需调度的**执行器**（访谈、反模式扫描、专项改造），不作为并行入口——两个 skill 不各自抢入口。impeccable 的命令对照、4 种 mode、调用纪律见 [references/impeccable.md](references/impeccable.md)。

阶段总纲见 [docs/ui-design.md](../../docs/ui-design.md)，判定真源见 [评审标准](../../docs/frontend-review-standard.md)，工程方法见 [通用方法](../../docs/frontend-engineering-methods.md)。加载其它 skill 时使用当前 coding agent 提供的 skill 加载机制；相关文件位于本仓 `skills/`。

## 交付定义（DoD）

| 块 | 必须做到 | 细则 |
|---|---|---|
| 工程化 | 复用项目脚手架与组件；tokens 单一真源（CSS 变量 / OKLCH，按项目支持明暗主题）；组件分层；运行项目提供的 lint、类型检查与构建 | [engineering.md](references/engineering.md) |
| 可观测 | 按项目接入浏览器日志、截图或性能观测；需要时检查版本更新与错误反馈 | [observability.md](references/observability.md) |
| 可维护 | 维护 DESIGN.md + tokens 决策记录；为 Agent 标明 UI 改动前先读设计契约；更新运行与交付说明；避免硬编码设计值 | [maintainability.md](references/maintainability.md) |

## Step 0 · 硬性前置：找项目设计真源

```bash
ls DESIGN.md docs/DESIGN.md design/DESIGN.md 2>/dev/null
rg -l -g '!node_modules' 'design-tokens|--color-|@theme' . | head
```

- 先读项目 `AGENTS.md` / `CLAUDE.md` 指定的设计文件、组件库或主题包，并遵循项目的品牌与协作规则。
- **先查 logo / brand-mark**：有已确认的品牌色时沿用；需要从 logo 推导色彩时遵循 [differentiation.md](references/differentiation.md#0-颜色来源铁律先查-logo--brand-mark)，并兼顾可读性和设计契约。
- **有 DESIGN.md / tokens** → 通读并服从；不重选风格或顺手改配色字体。与需求冲突时，若冲突会改变品牌或产品体验则澄清；否则按用户目标作最小一致调整并记录。
- **只有 tokens** → 从 tokens + 现有页面反推 DESIGN.md，标注为推断并继续任务；如用户要求方案评审，再提交确认。
- **都没有** → Step 1 确定方向。用户授权实现时可由 Agent 根据需求与证据选择并记录方向，再开始 UI 实现。

**存量项目补齐缺口**：先体检与任务相关的设计契约、tokens、组件状态、可访问性与浏览器复查；按 [engineering.md](references/engineering.md) 只补确实缺失的能力，**不推翻已有风格与工具链**。

## Step 0b · 存量反推（已有代码、缺 DESIGN.md）

项目**已有可运行的 UI 代码**但没有 DESIGN.md 时，方向不是从零拍脑袋，而是从代码反推：

1. **抽** → 调 impeccable `extract`：从现有代码抽 tokens（颜色、字阶、间距、圆角、阴影）与重复组件模式。命令、产出与调用方式见 [references/impeccable.md](references/impeccable.md)。
2. **成稿** → 用抽出的结果拼出 DESIGN.md 初稿：产品语气和方向可从页面与用户需求推断；无法推断的部分标注假设或待定。
3. **核对** → 用真实页面与用户目标对照反推结果，识别明显冲突。用户已授权实现时，除非发现无法逆转的重大品牌或产品决策，否则继续落实；需要用户参与评审时提供预览和取舍。
4. **落盘** → 按 [templates/DESIGN.md](templates/DESIGN.md) 结构落盘 `DESIGN.md` + tokens，决策记录区分「用户明确给定」与「根据存量代码/需求推断」。impeccable `document` 的结果也须校核后再采纳。

只有 tokens 没有 DESIGN.md 时同样走这条路（`extract` 的输入可以是 tokens + 页面）。

**新项目不适用本步**：没有代码可反推，直接 Step 1 定方向（差异化 + GEO）。

## Step 1 · 定方向（新项目 / 无 DESIGN.md / 明确改版）

1. **受众、目的、语气**：从用户目标、产品现状和页面内容确认或推导，并明确关键假设。仅当缺失信息会改变核心产品/品牌方向且不能从现有材料判断时才请求补充。impeccable `shape` / `init` 可辅助访谈和整理 brief。
2. **竞品扫描**：3–5 个同类产品，写「它们长什么样 → 我们刻意不同在哪」（已有 logo 时色彩从 logo 推导，无 logo 才从竞品/方向推导）。见 [differentiation.md](references/differentiation.md)。
3. **GEO 与内容结构**：此时就定信息架构、一句话定位、标题层级、FAQ、渲染方式、结构化数据。见 [geo.md](references/geo.md)。后台/内部工具可声明不适用，须写理由。
4. **形成方向并执行**：在内部比较 2–3 个有明显取舍的候选，用差异化与换名测试排除模板化方案；用户要求参与选型时，给出候选和预览。用户已授权实现时，选择最适合目标的一案，记录取舍后继续。

素材库（不是强制皮肤，按需从本仓 `skills/` 加载）：[apple-design](../apple-design/SKILL.md)（交互与动效原理）、[award-creative-ui](../award-creative-ui/SKILL.md)（创意类项目）、[landing-page-design](../landing-page-design/SKILL.md)（营销页结构与风格菜单）、`logo-brand-designer`（标识与品牌色）。

## Step 2 · 落盘：DESIGN.md + tokens

- 项目根写 `DESIGN.md`，用 [templates/DESIGN.md](templates/DESIGN.md)：定位、语气、定位与差异化、视觉系统、Do/Don't、参考、GEO/内容结构、决策记录（日期、证据、假设或取舍）。
- 写 tokens：颜色（light/dark 语义层）、字阶、间距、圆角、阴影、动效。示例 [templates/tokens.css](templates/tokens.css)；已有 Tailwind/主题包就落进它的配置，不另起一套。
- 项目 `AGENTS.md` 加入 UI 改动片段（见 [maintainability.md](references/maintainability.md#agentsmd-片段)）。
- impeccable `document` 生成的 DESIGN.md 需按本模板结构与项目事实核对后再写入；无法确认的决定标注为假设。

## Step 2.5 · 工程接线

适用项目先读 [engineering.md](references/engineering.md) 并检查现有脚手架、CI 与观测工具；只补项目确实缺少的能力。Vite 项目可使用 `npx ui-craft setup --dry-run` 检查 CLI 接线方案（已有仓库仍可使用 `contrast-guard` 命令别名），再按项目需要接入检查脚本。其它框架按 [engineering.md](references/engineering.md#非-vite-项目手动等价方案) 使用对应等价方式。

## Step 3 · 实现：只用 tokens

- 颜色、字号、间距、圆角、阴影、动效**只引用 tokens**；tokens 之外不出现裸 hex / rgb / px 魔数（`1px` 边线、栅格结构尺寸按项目约定）。
- 新 token：先改 tokens + DESIGN.md 决策记录补一行，再用。不在组件里就地造。
- 复用项目组件；hover / focus-visible / active / disabled / loading / empty / error 与视觉同时实现。
- 不加 DESIGN.md 未声明的风格元素（新渐变、新字体、新插画风）。

## Step 4 · 验收：项目规范 + 质量底线

先对照**本项目 DESIGN.md**（Do/Don't、可记忆点、语气），再过底线：

| 底线 | 证明方式 |
|---|---|
| 层级 / 间距 | 与本项目用户任务、信息量和视觉契约相符；避免默认堆卡片，间距优先复用项目 scale |
| 对比度 | 遵循项目已有目标；色彩 CLI 是可选辅助项，不代表整页已通过 |
| 反模式 | 在可用时用 `impeccable detect` 辅助发现模板化结构；必须结合真实截图和产品上下文判断 |
| 交互状态 | 相关交互可用键盘操作、focus 清晰，必要状态有设计 |
| 响应式 / 主题 | 声明的临界宽度无关键溢出遮挡；项目支持主题切换时检查相应主题 |
| GEO | [geo.md](references/geo.md)：对外公开页面检查内容是否便于搜索与 AI 摘要理解；后台或内部工具可注明不适用 |
| 工程 / 可观测 | 遵守项目规范；真实页面可检查，新增运行错误和未测范围如实记录 |

阶段路由：

| 阶段 | 按需 skill | 退出证据 |
|---|---|---|
| 1 定义任务 | 项目规范、[评审标准](../../docs/frontend-review-standard.md) | 设计契约；区分新设计/还原/缺陷修复 |
| 2 设计或还原 | 本 skill Step 1–2；有稿 [ui-restore](../ui-restore/SKILL.md)；整站参考 [clone-website](../clone-website/SKILL.md) | DESIGN.md + tokens 或还原方案 |
| 3 实现 | [apple-design](../apple-design/SKILL.md)、[design-to-motion](../design-to-motion/SKILL.md)、[award-creative-ui](../award-creative-ui/SKILL.md) | 可运行实现、影响面 |
| 4 比对修复 | [ui-restore](../ui-restore/SKILL.md)、[ui-review](../ui-review/SKILL.md) | 观察→原因→改法→前后证据 |
| 5 验收 | [frontend-e2e](../frontend-e2e/SKILL.md)、[browser-verify](../browser-verify/SKILL.md) | PASS/HOLD/未验证 + 截图 |
| 6 交付 | 项目交付规则 | 测试、部署版本、复测、残余项 |

工具不可用或换名测试未通过时如实标缺口并写清代价，不得用必然通过的假断言、HTTP 200 或本地通过冒充目标环境验收。

## 汇报（必填）

最终回复必须包含：

```text
UI 依据：已加载 skills = ui-design, <其它>；DESIGN.md = <路径@commit / 本次基于证据新建 / 不适用及原因>；真实截图/交互验证 = <证据或缺口>；残留问题 = <无 / 列表>
```

没有 DESIGN.md 却改了 UI = 违反本 skill；说明原因并补 Step 1/2。
