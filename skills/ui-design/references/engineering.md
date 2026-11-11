# 工程化细则

[ui-design](../SKILL.md) 的工程约定。遵循项目已经采用的框架、组件、检查和发布工具；ui-craft 不要求全仓迁移到特定脚手架。

## 通用清单

| 项 | 要求 | 建议做法 |
|---|---|---|
| 设计 token | 颜色、字阶、间距、圆角、阴影和动效尽量使用项目单一真源 | CSS variables、主题配置或项目现有 design tokens |
| 组件分层 | 页面复用项目基础组件；业务组件和页面职责清楚 | tokens → 基础组件 → 业务组件 → 页面，按项目已有结构调整 |
| 状态覆盖 | hover、focus、active、disabled、loading、empty、error 有明确呈现 | 将状态作为组件交付的一部分，不只做默认态 |
| 工程质量 | 运行项目已有 lint、类型检查和构建 | 不重复引入同类工具，不凭空增加检查门禁 |
| 可访问性 | 键盘操作、可见焦点、语义、文字对比度符合项目目标 | 色彩检查可用可选 CLI 辅助；实际页面仍需浏览器验证 |
| 发布与运行 | 遵循项目的 scripts、环境和上线流程 | 在项目 AGENTS / README / CI 中查证真实约定 |

## 可选 CLI

色彩配对检查和 Vite 接线均为可选能力。先在项目根目录查看 dry-run 的文件变更和依赖：

```bash
npx ui-craft setup --dry-run
```

只有输出与项目当前依赖、Vite 插件和 scripts 确认兼容时才执行 `npx ui-craft setup`。setup 可能安装 CLI/插件并改写配置；不要对已安装同类能力的项目盲目重复接线。静态色彩检查也可单独使用：

```bash
npx ui-craft --init
npx ui-craft check
```

完整能力边界见 [CLI 参考](../../../docs/contrast-cli.md)。旧项目可继续使用 `contrast-guard` 命令别名。

## 非 Vite 项目

非 Vite 项目仍可使用 skills 和 `check` CLI，不需要执行 `setup`。由项目现有工具负责组件引入、主题、遥测、版本更新检测、构建和部署；不要为采用 ui-craft 而替换已工作的工程链路。

## 存量项目：补齐缺口

先读设计契约和现状，只补与任务相关且确实缺失的内容，不推翻已有风格与工具链：

```text
[ ] DESIGN.md       [ ] tokens 单一真源    [ ] 组件状态完整
[ ] 键盘与响应式    [ ] lint / 类型 / 构建  [ ] GEO（或声明不适用）
[ ] 截图复查         [ ] 项目发布流程已确认
```

只在补充内容对项目未来开发有长期价值时更新 DESIGN.md 或 CHANGELOG；临时任务不强制创建全套设计系统。
