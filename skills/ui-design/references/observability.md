# 可观测细则

[ui-design](../SKILL.md) 交付定义的「可观测」部分。目标：页面出问题时 Agent 读证据定位，而不是猜代码。

| 项 | 要求 | 工具 / skill |
|---|---|---|
| 运行时遥测 | dev 期 API / 错误 / 控制台 / 交互日志、截图、DOM 快照、脱敏登录态 | vite-plugin-agent-eyes，`cs skill show agent-eyes` |
| 版本更新检测 | 部署后旧页面能发现新版本；旧 chunk 404 时兜底刷新，防白屏 | vite-plugin-refresh-guard，`cs skill show refresh-guard` |
| 错误上报 / 通知 | 生产环境前端错误需要被人看到时接上报；需要推送到飞书/企业微信时用 im-notify-kit（带重试、超时、去重） | im-notify-kit（owner 自有库，`cs repo im-notify-kit`） |
| 性能基线 | LCP / INP / CLS 有一次基线记录（页面 + 视口 + 网络条件），改版后复测对比 | Chrome DevTools / web-vitals；`cs skill show debug-optimize-lcp`（如已安装） |
| 视觉基线 | `contrast-guard measure <url> --save <name>` 存基线，改版后 `--vs <name>` 对比 | 本仓 CLI |

## 原则

- 遥测只记脱敏信息：不存 token、cookie、表单真实值。
- 生产上报要采样与去重，不把每个控制台 warning 都推给人。
- 没接上报的项目，在 DESIGN.md 或 README 写明「生产错误靠什么发现」，不能是空白。
- 验收时读遥测确认无新增错误（`log/<port>/` 下的日志），再交给 [browser-verify](../../browser-verify/SKILL.md) / [frontend-e2e](../../frontend-e2e/SKILL.md) 做截图与交互取证。
