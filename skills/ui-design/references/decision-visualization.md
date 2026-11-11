# 决策可视化细则

[ui-design](../SKILL.md) Step 0b / Step 1 及重大视觉变更拍板的执行细则。目标：**让 owner 先看到、摸到真实页面，再看图拍板**——不让 owner 凭空读文字猜 UI。

## 1. 何时触发与谁来做

- **触发时机**：任何需要 owner 拍板的 UI 变更，包括：
  - Step 0b 存量反推初稿确认；
  - Step 1 新项目 / 改版方向 2–3 个候选方案拍板；
  - 既有 `DESIGN.md` 边界之外的视觉调整、重设计或配色/版式变更。
- **执行主体**：执行当前 UI 任务的 Agent。在产出预览与候选对比后、向 owner 请求拍板**之前**必须完成「通道 A + 通道 B」双交付；owner 拍板前不得继续后续实现，也不得代 owner 拍板。

## 2. 通道 A：Owner Mac 真实预览（ego-lite / `ego-browser`）

先在 owner 的 Mac 上把真实可交互预览打开，让 owner 直接点击、滚动、切主题或比对断点。

### 可执行步骤

1. **启动并核验预览服务**
   - 确认本地 dev/preview server 可访问（记录真实 URL 与端口，例如 `http://127.0.0.1:4173/`）。
2. **用 `ego-browser` 打开 `taskSpace` 并截图、`handOff()`**
   - 通过 `ego-browser nodejs` 脚本创建 `taskSpace`，打开候选页面（多方案/双主题用 `task.newPage()` 分别打开），截取真实渲染图后调用 `task.handOff()` 移交控制权：

```js
import { taskSpace } from "ego-browser";

const task = await taskSpace("<项目>: UI 变更预览");
const p1 = task.page("p1");
await p1.goto("http://127.0.0.1:<port>/<route-a>");
await p1.screenshot({ path: "/abs/path/to/option-a.png", fullPage: true });

// 若有多个候选方案或暗色模式对比，再开页并截图
const p2 = await task.newPage();
await p2.goto("http://127.0.0.1:<port>/<route-b>");
await p2.screenshot({ path: "/abs/path/to/option-b.png", fullPage: true });

// 必须调用 handOff() 把页面留给 owner 实操；严禁调用 task.finish()（会关掉页面）
await task.handOff();
```

### 截图存放约定

- **本地绝对路径**：`page.screenshot({ path })` 必须传入**绝对路径**。统一放在：
  - `~/.local/state/cs-ui-decisions/<project>/<YYYY-MM-DD>-<slug>/`（推荐，不污染项目 git 工作区），或
  - 项目内临时目录 `.agent-artifacts/ui-decisions/<YYYY-MM-DD>-<slug>/`（须确保被 `.gitignore` 忽略，不得把临时对比截图提交进业务源码）。
- **跨端可访问 URL（给通道 B 决策卡用）**：
  - 优先发布/映射到项目预览服务静态路径或对象存储 HTTP(S) URL；
  - 若只有本机预览服务，提供同一预览服务的 HTTP URL（并写明访问边界，见第 4 节）。

## 3. 通道 B：`cs-ui-web` 决策收件箱带图决策卡

在完成通道 A 预览与截图后，立即在 `cs-ui-web` 决策收件箱创建一张带截图的决策卡，让 owner 在 Web-UI 或手机 `cs-app` 上直接看到图再拍板。

### 建卡命令（`cs decision` CLI）

```bash
cs decision create \
  --project "<项目标识>" \
  --by-agent "<agent_id>" \
  --task-key "<task_key>" \
  --stop-item "direction" \
  --title "<一句话说明要拍什么，如：选定首页改版方向 A/B>" \
  --options "<选项A（简要取舍）>,<选项B（简要取舍）>" \
  --detail "<方案对比摘要 + ego-lite 预览入口 + 截图 URL 链接>"
```

### 图片字段与当前降级方案

- **目标态（`decisions.images` 列）**：决策卡原生渲染图片依赖数据库 `decisions.images` 列（**生产库迁移待 owner 授权**）。该列及 CLI/API 传图参数可用后，按对应字段写入截图 URL 列表。
- **当前降级方案（`decisions.images` 列可用前）**：在 `--detail` 详情正文里直接贴出**图片 URL 链接**（按方案名、视口/主题与对应预览地址分段写清），保证 owner 在 `cs-ui-web` 决策页和手机 `cs-app` 点开即见图：

```text
ego-lite 预览：已通过 taskSpace + handOff() 在 owner Mac 打开（页面 p1/p2）
预览地址：http://127.0.0.1:<port>/...

候选对比与截图（decisions.images 待迁移，暂用 detail 图片链接降级）：
- 选项 A（...）：https://<static-or-preview-url>/option-a.png
- 选项 B（...）：https://<static-or-preview-url>/option-b.png
```

### 读取裁决与落盘

- owner 拍板后，Agent 通过 `cs decision inbox` / `cs decision show <id>` 读取选择并执行 `cs decision ack <id>`；
- 随后把结果写入项目 `DESIGN.md` 第 9 节「决策记录」（日期、拍板人、决策单 ID、放弃项及原因）。

## 4. 失败降级（禁止静默、禁止伪造）

| 故障场景 | 降级动作 | 严禁行为 |
|---|---|---|
| `ego-browser` 不可用（无桌面环境 / daemon 未启动 / 调用报错） | 改用本地可用的无头浏览器、`contrast-guard measure` 或 DevTools 截图；**仍须创建带图决策卡**，在 `--detail` 与汇报「DoD 缺口」写清 `ego-lite 预览未打开：<具体原因>` | 严禁伪造「已为 owner 打开真实预览」 |
| 仅有本地文件路径或 `127.0.0.1` 地址，暂无手机端可访问的 HTTP(S) URL | `--detail` 同时写明本地截图绝对路径与本地预览 URL，并注明「手机端无法直连本地地址，请在 Mac 本地预览或同局域网查看」 | 严禁假装手机 `cs-app` 已能正常拉取该图 |
| `decisions.images` 列尚未迁移或当前环境不支持传图字段 | 将截图 URL 链接写入 `--detail` 正文，并标注「当前用 detail 链接降级（`decisions.images` 待生产库迁移授权）」 | 严禁只发纯文字选项而省略截图信息 |
| `cs decision` CLI 或决策存储不可用 | 在当前会话直接输出候选对比、取舍、预览地址与截图路径/URL，并标明 `决策卡创建失败：<报错>` | 严禁宣称「已发出决策卡等待拍板」 |
