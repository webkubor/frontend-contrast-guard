# CLI 参考：可选的 UI 质量检查工具

ui-craft 的主价值是帮助 Agent 设计并改进 UI 的 skills。CLI 是可选工具，不能代替实际页面设计、截图检查或视觉判断。

## 安装

在项目里安装 CLI：

```bash
npm install -D github:webkubor/ui-craft
```

CLI 统一使用 `ui-craft` 命令。

## 检查已声明的色彩配对

```bash
npx ui-craft --init
npx ui-craft check
npx ui-craft check --json
```

配置文件 `contrast.config.json` 声明扫描文件与文字/背景配对。它检查的是已配置的颜色值组合，不会自动发现完整设计系统中的所有颜色，也不会证明真实叠层、图片背景、交互状态或整体 UI 无障碍全部合格。

## 页面样式采样

```bash
npx ui-craft measure https://example.com --save baseline
npx ui-craft measure http://localhost:5173 --vs baseline
npx ui-craft measure --baselines
```

`measure` 需要运行环境已有可用浏览器驱动。它提供 computed-style 分布观察，不是完整响应式审计或美感评分；样本变化需要结合页面和组件上下文解释。

## 记录 UI 评审

```bash
npx ui-craft review --init ui-review.json
npx ui-craft review ui-review.json --json
```

评审记录包含产品契约、声明检查范围、七个视觉维度、发现与可比的前后验证。命令读取 JSON 记录，不会自动打开页面、采集截图或分析图片。PASS 只表示记录满足门禁，不代表机器判断页面好看。

细则见 [评审 CLI](review-cli.md) 与 [UI 评审标准](frontend-review-standard.md)。

## 可选 Vite 接线

```bash
npx ui-craft setup --dry-run
npx ui-craft setup
```

setup 面向 Vite 工程，执行前先检查 dry-run 输出与项目当前插件、依赖和 scripts。已有配置不覆盖；项目可保留原有 CLI 命令与脚本名。
