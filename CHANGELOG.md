# Changelog

格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

### Added

- **`ui-design` 新增决策可视化、logo 导色与严格换名测试三条规则**（2026-11-11，owner 拍板）：
  - **决策可视化**（`skills/ui-design/references/decision-visualization.md`）：任何需 owner 拍板的 UI 变更，须先在 owner Mac 用 ego-lite（`ego-browser` `taskSpace` + `handOff()`）打开真实预览并截图，同时通过 `cs decision create` 在 `cs-ui-web` 决策收件箱产出带图决策卡（`decisions.images` 生产库迁移待 owner 授权，可用前用 `--detail` 图片 URL 链接降级）；含谁在何时做、截图存放与失败降级矩阵。
  - **颜色来自 logo**（`skills/ui-design/references/differentiation.md`、`SKILL.md`、`templates/DESIGN.md`）：项目已有 logo / brand-mark 时整站色彩必须从 logo 推导，不得另起色系（含 `cs-ui-web` `brand-mark.svg` 与 `blog` `logo.svg` 例证）；无 logo 时才走竞品/方向推导。
  - **换名测试收紧与失败处置**（`skills/ui-design/references/differentiation.md`、`SKILL.md`、`templates/DESIGN.md`）：至少在配色结构、版式、密度/信息结构三轴中两轴不同才通过，一轴即未通过；不通过必须诚实写「未通过」并写清代价，严禁掩盖或编写必然通过的假断言。
- **`ui-design` 接入 impeccable 作为执行器**（`skills/ui-design/references/impeccable.md`）：新增 Step 0b「存量反推」——项目已有代码但缺 DESIGN.md 时，先用 impeccable `extract` 抽 tokens 与组件模式 → 出初稿 → owner 确认 → 按 `templates/DESIGN.md` 落盘；新项目仍走 Step 1 定方向（差异化 + GEO）。命令 → 环节对照表（shape/init/extract/document/audit/critique/polish/harden/typeset/colorize/layout/adapt/optimize/detect）写明何时用、产出是什么、谁是真源；含 impeccable 4 种 mode（Persuade/Operate/Read/Experience）如何决定风格力度，以及「The brief wins」「Refinement preserves; Redesign replaces」两条底线。明确触发优先级：做 UI 先加载 `ui-design`，impeccable 由它按需调度，避免两个 skill 抢入口；impeccable 产出的 DESIGN.md 只是草稿，owner 确认后按 ui-design 模板落盘（唯一真源纪律不变）。Step 4 验收新增「反模式」底线并写清 `impeccable detect`（结构反模式扫描）与 `contrast-guard check`（色彩配对与计量）互补不重复，两者都跑。
- **`frontend-flow` 重建为 `ui-design` 2.0.0**（`skills/ui-design/`；`docs/frontend-flow.md` → `docs/ui-design.md`）：UI 设计与前端页面唯一入口，交付工程化、可观测、可维护的网站。Step 0 硬性前置先找项目 DESIGN.md / tokens，有则服从、不重选风格；没有则 Step 1 定方向（竞品扫描、可记忆点、换名测试、AI 模板味一票否决、GEO 内容结构、2–3 个带取舍候选由 owner 拍板）→ Step 2 落盘 DESIGN.md + tokens → Step 2.5 工程接线（`contrast-guard setup`）→ Step 3 只用 tokens 实现 → Step 4 按项目 DESIGN.md + 质量底线验收；存量项目走补齐缺口模式。附 `templates/DESIGN.md`、`templates/tokens.css` 与 `references/`（differentiation / geo / engineering / observability / maintainability）。其余 skills 的入口链接改为相对路径 `../ui-design/SKILL.md`。
- 新增 `contrast-guard setup [--dry-run]`：把前端工程体系引导装进 Vite 项目——安装本体（GitHub git 源，体系惯例不依赖 npm registry）与 vite-plugin-agent-eyes、vite-plugin-refresh-guard，备份后幂等改写 vite.config 注册插件，注入 `contrast:check` script，生成 `contrast.config.json` 模板，并把技能库落入项目 `.zcode/skills/`（随包 `skills/` 复制 + logo-brand-designer 克隆，已存在跳过不覆盖）。`--skip-install/config/scripts/skill/self` 分步执行；安装失败即中止，不改写 vite 配置。
- 新增前端研发 Flow（现为 [docs/ui-design.md](docs/ui-design.md)）：本仓为通用前端方法唯一真源——Flow 组织阶段、skills 提供按需方法、CLI 提供测量与证据检查；含六阶段路由原则与项目资产归属。
- 新增 `contrast-guard review --init ui-review.json` 与 `contrast-guard review ui-review.json --json`：以显式产品契约、七个评估维度、声明范围内的截图证据、具体改动和可比 before/after 复测记录形成评审门禁。
- 评估退出码：`0` 为记录满足门禁的 PASS，`1` 为 HOLD，`2` 为输入错误。初始化成功返回 `0`，空模板评估仍为 HOLD；初始化不覆盖已有文件。
- [规则演进](docs/rule-evolution.md)：规则身份与版本、正例与反例、`proposed / trial / stable / deprecated` 生命周期、试验预算、停止与回滚；配合更新后的 [前端 UI 评审标准](docs/frontend-review-standard.md)。

### Changed

- `measure` 默认返回中性 `observation`；缺少有效采样为 `unknown`。仅调用方显式提供阈值时给出配置提醒，不再把稀疏动效等分布当作通用审美失败。
- 双语 README 与 llms.txt 明确工具边界：`review` 读取 JSON，不自行采集或分析截图；PASS 不生成美感分数，也不证明截图已实际检查。四视口/主题/状态矩阵是声明的评审协议，不是 CLI 自动采集能力。
- 澄清 `measure` 提供 computed style 分布，不执行完整响应式审计、不计算真实渲染对比度，也不凭分布证明 token 漂移。

## [0.4.0] - 2026-10-25

### 🚀 重大进化：从单一配色护栏演进为「前端视觉全栈守卫」

承接 owner 2026-10-25 定调：收口对内前端工程与规范体系，CS 系统不再单独维护 UI 细节：
- **项目与包定位升级为 `frontend-contrast-guard`**：双别名兼容（支持 `frontend-contrast-guard` 与 `contrast-guard` 双命令行调用）。
- **沉淀前端工程视觉评审与交互自适应规范标准（SSOT）**：新增 `docs/frontend-review-standard.md`，承接原 CS 系统的核心视觉资产：
  - 视觉计量标准（computed style 采样分布，供情境化评审参考）；
  - 多视口响应式评审协议（四阶视口示例：390x844 / 768x1024 / 1280x800 / 1600x900，由评审者另行采集检查）；
  - 盒模型排版与组件三硬铁律（主标题特权律 Anti-Squeeze、全局浮层避让律 Anti-Occlusion、交互端到端真实闭环）；
  - 组件选型与弹窗铁律（坚决拒绝原生 alert/confirm/prompt，优先复用成熟组件库）；
  - 前端全生命周期工具流（插件类、CLI类、npm 研发库三大类资产协同流）。
- **重构 README**：首屏对齐开源金字塔规范（居中品牌、for-the-badge 徽章、置顶 Why This 痛点对比矩阵）。

## [0.3.0] - 2026-09-08

主发布：

- **新增 `measure` 子命令** —— 渲染后视觉计量（cf128c7）
- **`measure` 自驱动**：ego 驱动收掉自己开的 task space（0a9df0a）
- **失败即判死**：导航失败时不再把错误页的指标当页面指标（2bcda42）

### 文档

- 与 `vite-plugin-agent-eyes` 互链，说明前端质量链路分工（7890a5c）
- README 加 Bloom 系列导航（a3a0b48）

## 历史

- `0c7ce3a` — feat: 支持一个文件内多组色板（groups 模式）
- `a896a4c` — fix: 修复两个 dogfooding 暴露的缺陷 —— `@media` 覆盖污染与 hex 无建议值
- `6f9ce0e` — feat: contrast-guard 首版 —— 把配色对比度做成 CI 护栏，并直接给出该改成多少

---

完整 commit 列表见 `git log`。