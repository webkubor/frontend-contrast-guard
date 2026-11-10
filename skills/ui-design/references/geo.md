# GEO 细则（生成式引擎 / AI 搜索可见性）

[ui-design](../SKILL.md) Step 1 定方向时就要定内容结构；Step 4 按下方清单验收。基线与复测走 `cs skill show project-maturity-audit`（已有 GEO 基线与复测流程）。

## 适用范围

- 对外页面（官网、落地页、文档、博客、产品页）：**适用**。
- 后台 / 内部工具 / 登录后应用：可声明**不适用**，但必须在 DESIGN.md「GEO / 内容结构」写明理由（例：全部在登录后、不被索引）。

## 方向阶段要定的

1. **实体与一句话定位**：这是什么、给谁、解决什么——一句话出现在首屏可见文本里，和 `<title>`、meta description、JSON-LD 描述一致。
2. **信息架构**：页面清单与每页唯一 `h1`，`h2/h3` 按内容层级而非视觉大小选。
3. **渲染方式**：SSR / SSG / 预渲染优先；纯 CSR 必须写明方案（预渲染、动态渲染或接受不被抓取的理由）。
4. **问答块**：用户真实会问的 3–8 个问题，做成可见的 FAQ（不是折叠在 JS 里不出 DOM）。
5. **结构化数据**：按需 `Organization`、`Product` / `SoftwareApplication`、`FAQPage`、`Article`、`BreadcrumbList`（JSON-LD）。
6. **机器可读入口**：`llms.txt`、`sitemap.xml`、`robots.txt`、canonical。

## 视觉不得牺牲的

- 关键文案（定位、价格、功能、FAQ）必须是 DOM 文本，不能只在图片、canvas、SVG 路径或视频里。
- 动效/滚动叙事不能把内容藏到交互之后才挂载；首屏 HTML 里就要有。
- 图片有意义的给 `alt`；装饰图 `alt=""`。
- 性能与可访问性是 GEO 的一部分：LCP、CLS 合理，语义化地标（`header/nav/main/footer`）。

## 验收清单（Step 4）

- [ ] 每页唯一 `h1`，标题层级无跳级
- [ ] 一句话定位出现在首屏文本、`<title>`、meta description、JSON-LD 中且一致
- [ ] 关闭 JS 或查看源码时核心内容可见（SSR/预渲染生效）
- [ ] JSON-LD 通过校验（Rich Results Test 或 schema 校验器）
- [ ] FAQ 为可见 DOM 文本，且有对应 `FAQPage`（如适用）
- [ ] `llms.txt`、`sitemap.xml`、`robots.txt`、canonical、OG/Twitter meta 齐全
- [ ] 关键文案无"只在图片里"的情况
- [ ] 需要量化基线或复测时已跑 `project-maturity-audit` 的 GEO 部分并记录结果
