# impeccable 接入细则

[ui-design](../SKILL.md) 的**执行器**参考。impeccable 是外部 skill bundle（本机装在 `~/.claude/skills/impeccable`、`~/.agents/skills/impeccable`、`~/.cursor/skills/impeccable`、`~/.codex/skills/impeccable`、`~/.gemini/config/skills/impeccable`、`~/.config/opencode/skills/impeccable`、`~/.dsh/skills/impeccable`、`~/.hermes/skills/impeccable`），提供访谈、反模式扫描、专项改造等能力；**方向、风格与 DESIGN.md 真源始终在 ui-design 一侧**。

本页解决三件事：什么时候调 impeccable、每条命令产出什么、谁说了算。

## 0. 触发优先级（避免两个 skill 抢入口）

1. **做 UI 先加载 `ui-design`**。它是唯一入口，负责找真源、定方向、落盘 DESIGN.md、调度其它 skill。
2. **impeccable 由 ui-design 按需调度**，不作为并行入口。用户直接说「用 impeccable / audit 一下 / 反模式扫一下」时才直接调它。
3. impeccable 自带的 `SKILL.md` 描述很宽（design / redesign / critique / audit / polish / animate），容易和 ui-design 抢同一批词。已在其安装副本的 frontmatter 里加了一句作用域声明（不改动 `scripts/` 与 `reference/`）。**注意：`impeccable update` 可能覆盖这行声明，届时按本节手动恢复。**
4. 任何时候**只有一份 DESIGN.md 真源** = 项目里的 `DESIGN.md`，结构用 [templates/DESIGN.md](../templates/DESIGN.md)。impeccable 的 `document` 产出是**草稿**，未经 owner 确认不得直接落盘。

## 1. 调用方式

```bash
# 各 harness 内 skill 自带 launcher（用 skill 目录下那份，路径随 harness 变）
~/.claude/skills/impeccable/scripts/impeccable <command> [args]
```

- 引擎二进制随 bundle 版本固定（bundle v4.8.2 → engine v0.1.21），launcher 优先用本地 `scripts/bin/<os>-<arch>/`，其次用 `~/.impeccable/bin/<version>/`；两者都没有时才尝试联网下载。**离线环境必须先备好本地引擎**，否则只有 `NO_ENGINE` 类报错。
- `impeccable context` 先跑一次，拿到 `hasVisualImplementation` / `productPath` / `designPath`，再决定命令；`impeccable context` 在无 `PRODUCT.md` 时会提示走 `init`。
- 全局 `impeccable`（npm v3.6.0）是退役 CLI，只认 `skills` 旧命名空间；**本 skill 一律用 skill 目录里的 launcher**。

## 2. 命令 → 环节对照表

| 命令 | 何时用 | 产出 | 真源归属 |
|---|---|---|---|
| `shape [feature]` | 定方向**之前**的访谈：把需求、用户、边界问清 | 一份用户确认的 design brief（方向约束，不是 DESIGN.md） | brief 是 ui-design Step 1 的输入；方向仍由 owner 拍板 |
| `init` | 项目没有任何产品上下文（无 `PRODUCT.md`） | `PRODUCT.md`：用户、品牌、原则 | 战略上下文，非视觉真源；与 ui-design 的 DESIGN.md 分层 |
| `extract [target]` | **存量项目有代码、缺 DESIGN.md**：从现有代码抽 tokens 与重复组件模式 | tokens 清单 + 组件模式草案 | 草案输入；落盘走 ui-design 模板 |
| `document` | 需要把**现有**视觉系统写成规范 | `DESIGN.md` 草稿（Google Stitch 格式） | **草稿**；必须 owner 确认后按 ui-design 的 templates/DESIGN.md 落盘 |
| `audit [target]` | 实施后的技术质量审查：可访问性、性能、主题、响应式 | P0–P3 分级报告 | 与 ui-design Step 4 的底线表合并判读，不单独定风格 |
| `critique [target]` | 实施后的 UX 评审：层级、信息架构、认知负荷、情绪共鸣 | 启发式评分 + 问题清单 | 同上，结论不覆盖项目 DESIGN.md 的 Do/Don't |
| `polish [target]` | 上线前最后一轮：对齐、间距、一致性、微细节 | 微调后的实现 | 只在 DESIGN.md 范围内收敛，不新增风格元素 |
| `harden [target]` | 生产化：错误处理、i18n、文本溢出、边界数据 | 健壮性修复 | 与 ui-design「状态齐全」底线对齐 |
| `typeset [target]` | 字体、字阶、行高、可读性问题 | 排版修正 | 字号字阶只允许引用 tokens，不就地造 |
| `colorize [target]` | 界面过于单色、缺视觉重点 | 配色调整 | 颜色只允许引用 tokens；改动需同步 tokens + DESIGN.md 决策记录 |
| `layout [target]` | 网格单调、间距不一致、层级弱 | 布局/间距修正 | 间距只来自 scale |
| `adapt [target]` | 响应式：断点、流式布局、触控目标 | 多尺寸适配实现 | 与 DESIGN.md 声明的临界宽度一致 |
| `optimize [target]` | UI 性能：加载、渲染、动画、图片、包体 | 性能诊断与修复 | 与 ui-design DoD「可观测」块的 LCP/INP/CLS 基线一致 |
| `detect [file-or-dir]` | **静态反模式扫描**（本地 HTML/CSS，无网络、web-only） | 反模式命中列表（JSON） | 见 §4：与 contrast-guard 互补 |

其余命令（`bolder` / `quieter` / `distill` / `delight` / `overdrive` / `clarify` / `onboard` / `animate` / `live` / `generate`）属**风格与体验侧**，只在 ui-design 已定方向、且 owner 明确要某个力度的调整时按需调；`live` / `generate` / `detect` 只适用于 web，原生端不适用。

## 3. 4 种 mode 决定风格力度

impeccable 用 mode 决定「敢做多狠」。选错 mode 是最常见的风格事故来源：

| Mode | 适用场景 | 力度 | 风险 |
|---|---|---|---|
| **Persuade** | 营销页、首发、品牌叙事 | 全力投入：鲜明、有观点 | 容易过度设计 |
| **Operate** | 后台、工具、工作台（用户来干活） | 世界只贡献**字体、色板、密度、一个标志性动作**；布局与控件保持品类标准 | 容易把产品做成「世界里的道具」 |
| **Read** | 阅读、文档、内容型 | 克制，版式与可读性优先 | 容易显得平淡 |
| **Experience** | 消费级、沉浸式体验 | 全力投入 + 微交互 | 容易与功能冲突 |

判据：**这个界面是让访客「记住」还是「办事」**。工作台/后台默认 `Operate`，营销/品牌页默认 `Persuade`。mode 决定幅度，不决定色板和字体——色板和字体仍由项目 DESIGN.md 定。

## 4. 两条底线

- **The brief wins（brief 优先）**：用户/owner 的 brief、需求、约束压过 impeccable 的任何默认审美。impeccable 的默认风格不是风格真源；与 brief 冲突时以 brief 为准，并说明取舍。
- **Refinement preserves; Redesign replaces（精修保留，重设计替换）**：`polish` / `typeset` / `layout` / `colorize` / `harden` / `adapt` / `optimize` 是**精修**——在既有视觉系统内收敛，不换方向、不重选色板；`shape` / `init` 属**重设计**——可以换方向，但必须重新走 ui-design Step 1 并由 owner 拍板。

## 5. `impeccable detect` vs `contrast-guard check`

两者互补，不重复，都跑：

| | `impeccable detect` | `contrast-guard check` |
|---|---|---|
| 扫什么 | **UI 反模式**：渐变文字、emoji 图标、堆叠大圆角卡片、模板化 hero 等「AI 味」结构 | **色彩配对与计量**：声明的 token 组合是否达标 WCAG AA/AAA，OKLCH 精度，不达标给修复值 |
| 输入 | 本地 HTML / CSS 文件（静态，web-only） | `contrast.config.json` 里的变量组合 |
| 能力 | 无网络、不需要 dev server | 可进 CI，退出码语义 0 通过 / 非 0 失败 |
| 缺口 | 不量对比度 | 不看结构反模式 |

配套还有 `contrast-guard measure`：渲染后量真实色值与视觉程度，用于人工抽查（自动化只能静态推断）。

## 6. 调用顺序建议

```text
Step 0  找 DESIGN.md / tokens
          └─ 无 DESIGN.md 但有代码 → Step 0b：extract → 初稿 → owner 确认 → 按 templates/DESIGN.md 落盘
          └─ 无代码 → Step 1：shape/init 访谈 → 差异化 + GEO → owner 拍板
Step 2  落盘 DESIGN.md + tokens
Step 3  实现（只用 tokens）
Step 4  验收：impeccable detect（反模式） + contrast-guard check/measure（色彩）
          └─ 有问题再按需调 polish / typeset / layout / harden / adapt
```

## 7. 纪律

- 不改 impeccable 上游的 `scripts/` 与 `reference/` 内容；不把它的文件搬进本仓。
- impeccable 产出的任何文档都是**草稿**，落盘前必须 owner 确认，且用 ui-design 的模板结构。
- 工具报错就标缺口，不用「本地通过」冒充真实环境验收。
