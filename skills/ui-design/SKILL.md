---
name: ui-design
version: 2.0.0
description: "UI 设计与前端页面开发的唯一入口，交付工程化、可观测、可维护的网站而不只是好看的页面：先找项目 DESIGN.md / tokens，有就照做不重选风格；没有就先定方向（竞品扫描、差异化、GEO 内容结构、2–3 个带取舍的候选由 owner 拍板），落盘 DESIGN.md + tokens，接线工程与遥测，只用 tokens 实现，按项目规范 + 质量底线验收。存量项目走补齐缺口模式。触发：UI、设计、页面、样式、配色、改版、前端页面、视觉风格、设计规范、新页面、重设计、主题、暗色模式、建站。"
license: MIT
---

# UI Design

## 总纲

这是 UI skill，但**交付物是工程化、可观测、可维护的网站**，不是一张好看的页面；细节由本 skill 替 owner 处理好。

- **跨项目风格自由**：不同项目应该长得不一样，本 skill 不带默认皮肤。
- **项目内风格一致**：风格真源是**项目自己的 DESIGN.md + tokens**，不是 Agent 当下的审美。改 UI 前先找它，没有就先定方向，不准自己编风格。
- **质量底线统一**：层级、间距、对比度、交互状态、响应式、GEO、工程接线，所有项目同一条线。

阶段总纲见 [docs/ui-design.md](../../docs/ui-design.md)，判定真源见 [评审标准](../../docs/frontend-review-standard.md)，工程方法见 [通用方法](../../docs/frontend-engineering-methods.md)。加载其它 skill 一律 `cs skill show <name>`（计入 `cs skill stats`）。

## 交付定义（DoD）

| 块 | 必须做到 | 细则 |
|---|---|---|
| 工程化 | Vite 优先脚手架；tokens 单一真源（CSS 变量 / OKLCH，明暗主题）；组件分层；lint + 类型检查；构建产物检查；`contrast-guard setup` 一键接线；agentGit / agentGuard 提交期门禁（`agent-eyes`） | [engineering.md](references/engineering.md) |
| 可观测 | `agent-eyes` 运行时遥测（API/错误/控制台/交互、截图、DOM 快照）；`refresh-guard` 版本检测防旧 chunk 白屏；需要时错误上报/通知（im-notify-kit）；LCP/INP/CLS 基线 | [observability.md](references/observability.md) |
| 可维护 | DESIGN.md + tokens 决策记录；AGENTS.md 写入「UI 改动前先读 DESIGN.md 并 `cs skill show ui-design`」；README 运行/部署说明；CHANGELOG/版本（`release-sync`）；无超长文件（agentSizeWatch）；无硬编码颜色/尺寸 | [maintainability.md](references/maintainability.md) |

## Step 0 · 硬性前置：找项目设计真源

```bash
ls DESIGN.md docs/DESIGN.md design/DESIGN.md 2>/dev/null
rg -l -g '!node_modules' 'design-tokens|--color-|@theme' . | head
```

- 先读项目 `AGENTS.md` / `CLAUDE.md` 指定的设计文件、组件库或主题包（公司项目走 `company-frontend-flow` 与其 tokens/组件库）。
- **有 DESIGN.md / tokens** → 通读并服从，**不重选风格、不顺手改配色字体**；与需求冲突问 owner。进入「存量补齐」或直接 Step 3。
- **只有 tokens** → 从 tokens + 现有页面反推 DESIGN.md，标注「反推，待 owner 确认」。
- **都没有** → Step 1；**owner 选定方向前不写 UI 代码**。

**存量项目补齐缺口**：先体检缺哪些（DESIGN.md / tokens / contrast 门禁 / agent-eyes / refresh-guard / agentGuard / GEO），按 [engineering.md](references/engineering.md#存量项目补齐缺口) 清单只补缺失项，**不推翻已有风格与工具链**。

## Step 1 · 定方向（新项目 / 无 DESIGN.md / 明确改版）

1. **受众、目的、语气**：问清或推导；以及设备与主题要求。
2. **竞品扫描**：3–5 个同类产品，写「它们长什么样 → 我们刻意不同在哪」。见 [differentiation.md](references/differentiation.md)。
3. **GEO 与内容结构**：此时就定信息架构、一句话定位、标题层级、FAQ、渲染方式、结构化数据。见 [geo.md](references/geo.md)。后台/内部工具可声明不适用，须写理由。
4. **2–3 个差异明显的候选方向**，每个含：立意 + 参考；色板 / 字体字阶 / 密度 / 圆角阴影 / 动效草图；**取舍（放弃了什么）**；一个**可记忆点**；通过**换名测试**与 **AI 模板味检查**。
5. **owner 拍板**，Agent 不得自选；未回复前停在本步。

素材库（不是强制皮肤，按需 `cs skill show`）：[apple-design](../apple-design/SKILL.md)（交互与动效原理）、[award-creative-ui](../award-creative-ui/SKILL.md)（明确创意类）、[landing-page-design](../landing-page-design/SKILL.md)（营销页结构与风格菜单）、`logo-brand-designer`（标识与品牌色）、owner 的 Bloom OKLCH 色板（`dsh-bloom-theme` / `typora-Bloom-theme`）。

## Step 2 · 落盘：DESIGN.md + tokens

- 项目根写 `DESIGN.md`，用 [templates/DESIGN.md](templates/DESIGN.md)：定位、语气、定位与差异化、视觉系统、Do/Don't、参考、GEO/内容结构、决策记录（日期 + 拍板人 + 放弃项）。
- 写 tokens：颜色（light/dark 语义层）、字阶、间距、圆角、阴影、动效。示例 [templates/tokens.css](templates/tokens.css)；已有 Tailwind/主题包就落进它的配置，不另起一套。
- 项目 `AGENTS.md` 加入 UI 改动片段（见 [maintainability.md](references/maintainability.md#agentsmd-片段)）。

## Step 2.5 · 工程接线

Vite 项目：`npx contrast-guard setup --dry-run` 看改动 → `npx contrast-guard setup`（装 vite-plugin-agent-eyes + vite-plugin-refresh-guard、写 `contrast.config.json`、注入 `contrast:check`）→ `contrast-guard check`。再按 `cs skill show agent-eyes` 打开 agentGit/agentGuard，按 `cs skill show refresh-guard` 选更新强度（不确定就问 owner）。非 Vite 项目用 [engineering.md](references/engineering.md#非-vite-项目手动等价方案) 的手动等价方案。

## Step 3 · 实现：只用 tokens

- 颜色、字号、间距、圆角、阴影、动效**只引用 tokens**；tokens 之外不出现裸 hex / rgb / px 魔数（`1px` 边线、栅格结构尺寸按项目约定）。
- 新 token：先改 tokens + DESIGN.md 决策记录补一行，再用。不在组件里就地造。
- 复用项目组件；hover / focus-visible / active / disabled / loading / empty / error 与视觉同时实现。
- 不加 DESIGN.md 未声明的风格元素（新渐变、新字体、新插画风）。

## Step 4 · 验收：项目规范 + 质量底线

先对照**本项目 DESIGN.md**（Do/Don't、可记忆点、语气），再过底线：

| 底线 | 证明方式 |
|---|---|
| 层级 / 间距 | 一屏一个主焦点；间距全来自 scale |
| 对比度 | `contrast-guard check` 通过；渲染后 `contrast-guard measure` 抽查 |
| 交互状态 | 键盘可达、focus 可见、状态齐全 |
| 响应式 / 暗色 | 声明的临界宽度无溢出遮挡；声明暗色则双主题截图 |
| GEO | [geo.md](references/geo.md) 验收清单；基线/复测 `cs skill show project-maturity-audit` |
| 工程 / 可观测 | DoD 三块逐项勾选；遥测日志无新增错误 |

阶段路由：

| 阶段 | 按需 skill | 退出证据 |
|---|---|---|
| 1 定义任务 | 项目规范、[评审标准](../../docs/frontend-review-standard.md) | 设计契约；区分新设计/还原/缺陷修复 |
| 2 设计或还原 | 本 skill Step 1–2；有稿 [ui-restore](../ui-restore/SKILL.md)；整站参考 [clone-website](../clone-website/SKILL.md) | DESIGN.md + tokens 或还原方案 |
| 3 实现 | [apple-design](../apple-design/SKILL.md)、[design-to-motion](../design-to-motion/SKILL.md)、[award-creative-ui](../award-creative-ui/SKILL.md) | 可运行实现、影响面 |
| 4 比对修复 | [ui-restore](../ui-restore/SKILL.md)、[ui-review](../ui-review/SKILL.md) | 观察→原因→改法→前后证据 |
| 5 验收 | [frontend-e2e](../frontend-e2e/SKILL.md)、[browser-verify](../browser-verify/SKILL.md) | PASS/HOLD/未验证 + 截图 |
| 6 交付 | 项目交付规则、`release-sync` | 测试、部署版本、复测、残余项 |

工具不可用时如实标缺口，不用 HTTP 200 或本地通过冒充目标环境验收。

## 汇报（必填）

最终回复必须包含：

```text
UI 依据：已加载 skills = ui-design, <其它>；遵循 DESIGN.md = <路径@commit 或「新建，owner 于 YYYY-MM-DD 选定方向 X」>；DoD 缺口 = <无 / 列表>
```

没有 DESIGN.md 却改了 UI = 违反本 skill；说明原因并补 Step 1/2。
