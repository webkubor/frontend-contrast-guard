# 可维护细则

UI 工作的长期产物是可延续的设计契约、tokens 与可理解组件；遵守项目的既有文档和代码风格。

## 维护项

| 主题 | 约定 |
|---|---|
| 设计真源 | 新增长期有效的设计决策时更新 DESIGN.md 和 tokens；一次性小改动无需创建整套设计系统 |
| Agent 上下文 | 在项目 AGENTS.md / CLAUDE.md 中链接本项目 DESIGN.md，并说明 UI 改动前读取它 |
| 组件与状态 | 将重复的视觉和交互抽到项目组件；不要为每个页面复制一份近似实现 |
| 变更记录 | 按项目已有方式更新 changelog、版本和设计决策记录；本 skill 不规定项目发版工具 |
| 评审记录 | 视觉改动记录页面版本、截图条件、具体发现、修改和复查证据 |

## AGENTS.md / CLAUDE.md 参考片段

```markdown
## UI changes

- Before changing UI, read `DESIGN.md` and the referenced design tokens.
- Follow existing components and document any decision that changes the design system.
- After implementation, inspect the rendered page in a browser and fix important visual issues.
```

将文件路径和命令改成项目实际值。不要写入不可用的工具名称或声称已接入未安装的审计、版本同步或浏览器系统。

## 避免复制粘贴式设计

在页面或组件中发现重复的配色、字号、间距和状态规则时，先对照现有 tokens 与组件库；确有长期复用价值再抽象。保持改动与项目任务相称，不为使用 UI skill 而强制重构。
