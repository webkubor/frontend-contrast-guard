# 工程化细则

[ui-design](../SKILL.md) 交付定义的「工程化」部分，对应 Step 2.5「工程接线」。

## 清单

| 项 | 要求 | 工具 / skill |
|---|---|---|
| 脚手架 | Vite 优先（Vue/React/Astro 按项目）；目录约定写进 README | — |
| tokens 单一真源 | CSS 变量（OKLCH），light/dark 语义层；Tailwind/主题包也从同一份 tokens 派生 | [templates/tokens.css](../templates/tokens.css) |
| 对比度门禁 | 颜色对写进 `contrast.config.json`，`contrast-guard check` 进 CI | 本仓 CLI |
| 组件分层 | tokens → 基础组件（按钮/输入/卡片）→ 业务组件 → 页面；页面不直接写样式魔数 | — |
| lint / 类型 | ESLint（或项目既有）+ `tsc --noEmit` / `vue-tsc`；Stylelint 可选（禁裸 hex） | — |
| 构建产物检查 | `vite build` 通过；产物体积与 chunk 数有基线；无 sourcemap 泄露到生产（按项目约定） | — |
| 一键接线 | `contrast-guard setup`：装本体 + vite-plugin-agent-eyes + vite-plugin-refresh-guard，改写 vite.config，注入 `contrast:check`，生成 `contrast.config.json`，落技能库到 `.zcode/skills/` | 本仓 CLI，先 `--dry-run` |
| 提交期门禁 | agentGit（pre-commit/post-commit）+ agentGuard（拦 secret/超长文件/屎山信号） | `cs skill show agent-eyes` |

## 新项目：Step 2.5 工程接线

```bash
npx contrast-guard setup --dry-run   # 先看会改什么
npx contrast-guard setup             # 执行；失败即中止，不改 vite 配置
npx contrast-guard check             # 对比度门禁
```

然后按 agent-eyes skill 打开 agentGit / agentGuard，按 refresh-guard skill 选更新强度（三档不确定就问 owner）。

## 非 Vite 项目：手动等价方案

| Vite 能力 | 手动等价 |
|---|---|
| contrast-guard setup | `npm i -D github:webkubor/frontend-contrast-guard` + `contrast-guard --init`，在 CI 加 `contrast-guard check` |
| agent-eyes 遥测 | 自行接全局错误/未处理 rejection/API 失败日志 + 错误上报（见 [可观测细则](observability.md)） |
| agentGit / agentGuard | 项目既有 git hooks（lefthook/husky/pre-commit）跑 lint + 类型 + secret 扫描 + 文件长度检查 |
| refresh-guard | 构建时输出 `version.json`，客户端轮询比对；捕获 chunk 加载失败后强制刷新一次（带冷却防循环） |

## 存量项目：补齐缺口

先体检，只补缺失项，**不推翻已有风格与工具链**：

```text
[ ] DESIGN.md   [ ] tokens 单一真源   [ ] contrast.config.json + check
[ ] agent-eyes  [ ] refresh-guard      [ ] agentGit/agentGuard（或等价 hooks）
[ ] lint/类型   [ ] 构建通过           [ ] GEO（或声明不适用）
```

每补一项在 DESIGN.md 决策记录或 CHANGELOG 记一行。已有等价方案（如项目自带 husky + 自研版本检测）视为满足，不替换，除非 owner 要求。
