# Tauri React 模板

一个"开箱即用"的模板，用于构建生产就绪的桌面应用程序，基于 **Tauri v2**、**React** 和 **TypeScript**。采用固定的架构模式设计，帮助开发者和 AI 编码助手从一开始就构建架构良好的应用。

## 为什么选择这个模板？

大多数 Tauri 启动模板只给你一个空白画布。这个模板为你提供了一个**可运行的应用程序**，并已建立了完善的模式：

- **类型安全的 Rust-TypeScript 桥接**，通过 tauri-specta 实现
- **工具强制执行的性能模式** - 包含常规 linting 以及 ast-grep 检测常见反模式
- **多窗口架构**已经可用（带全局快捷键的快速面板作为演示）
- **跨平台就绪**，包含平台特定的标题栏、窗口控件和原生菜单集成
- **内置国际化**，支持 RTL（从右到左）布局

## 技术栈

| 层级     | 技术                                            |
| -------- | ----------------------------------------------- |
| 前端     | React 19, TypeScript, Vite 7                    |
| UI       | shadcn/ui v4, Tailwind CSS v4, Lucide React     |
| 状态管理 | Zustand v5, TanStack Query v5                   |
| 后端     | Tauri v2, Rust                                  |
| 测试     | Vitest v4, Testing Library                      |
| 质量保证 | ESLint, Prettier, ast-grep, knip, jscpd, clippy |

## 已实现的功能

该模板包含一个可运行的应用程序，已实现以下功能：

### 核心功能

- **命令面板** (`Cmd+K`) - 可搜索的命令启动器，支持键盘导航
- **快速面板** - 全局快捷键 (`Cmd+Shift+.`) 可从任何应用打开浮动窗口，即使在全屏模式下也可以。在 macOS 上使用原生 NSPanel 实现正确的全屏覆盖行为。
- **键盘快捷键** - 平台感知的快捷键，自动集成到菜单
- **原生菜单** - 从 JavaScript 构建的文件、编辑、查看菜单，完全支持国际化
- **偏好设置系统** - 设置对话框，Rust 端持久化，React hooks，以及全局类型安全访问
- **可折叠侧边栏** - 空的左右侧边栏，通过可调整大小的面板实现状态持久化
- **主题系统** - 明暗模式，支持系统偏好检测，跨窗口同步
- **通知** - 应用内反馈的 Toast 通知，以及原生系统通知
- **自动更新** - 配置了 Tauri 更新插件，集成 GitHub Releases，启动时检查更新
- **日志记录** - Rust 和 TypeScript 的结构化日志工具，格式一致
- **崩溃恢复** - 紧急数据持久化，用于在意外退出后恢复未保存的工作

### 架构模式

- **三层状态管理** - 清晰的决策树：`useState`（组件）→ `Zustand`（全局 UI）→ `TanStack Query`（应用不拥有的持久化数据）
- **事件驱动的 Rust-React 桥接** - 菜单、快捷键和命令面板都通过同一个命令系统路由
- **React Compiler** - 自动记忆化，无需手动使用 `useMemo`/`useCallback`

### 跨平台

| 平台    | 标题栏               | 窗口控件 | 打包格式      |
| ------- | -------------------- | -------- | ------------- |
| macOS   | 自定义，带毛玻璃效果 | 交通灯   | `.dmg`        |
| Windows | 自定义               | 右侧     | `.msi`        |
| Linux   | 原生 + 工具栏        | 原生     | `.AppImage`   |

平台检测工具、平台特定的 UI 字符串（"在访达中显示" vs "在资源管理器中显示"）以及每个平台的独立 Tauri 配置都已设置完成。

### 开发体验

- **类型安全的 Tauri 命令** - tauri-specta 从 Rust 生成 TypeScript 绑定，完全支持自动补全和编译时检查
- **静态分析** - ESLint、Prettier、ast-grep（架构强制）、knip（未使用代码）、jscpd（重复代码）
- **单一质量门** - `npm run check:all` 运行 TypeScript、ESLint、Prettier、ast-grep、clippy 和所有测试
- **测试模式** - Vitest 设置，支持 Tauri 命令模拟

## 包含的 Tauri 插件

| 插件              | 用途                       |
| ----------------- | -------------------------- |
| single-instance   | 防止多个应用实例           |
| window-state      | 记住窗口位置/大小          |
| fs                | 文件系统访问               |
| dialog            | 原生打开/保存对话框        |
| notification      | 系统通知                   |
| clipboard-manager | 剪贴板访问                 |
| global-shortcut   | 系统级键盘快捷键           |
| updater           | 应用内自动更新             |
| opener            | 使用默认应用打开 URL/文件  |
| tauri-nspanel     | macOS 浮动面板行为         |

## AI 友好的开发

该模板设计为与 AI 编码助手（如 Claude Code）良好配合：

- **全面的文档**，位于 `docs/developer/`，涵盖所有模式。人类可读，但实际上是为了向 AI 助手解释某些模式的"原因"而设计的。不是废话。
- **Claude Code 集成** - 自定义命令（`/check`、`/cleanup`）和几个专门的助手
- **合理的文件组织** - React 代码在 `src/` 中，清晰分离（组件、hooks、stores、services），Rust 在 `src-tauri/src/` 中，模块化的命令组织。对人类和 AI 都是可预测的结构。

## 开始使用

查看 **[使用此模板](docs/USING_THIS_TEMPLATE.md)** 获取设置说明和工作流程指导。

### 快速开始

```bash
# 前置要求：Node.js 18+, Rust（最新稳定版）
# 查看 https://tauri.app/start/prerequisites/ 了解平台特定依赖

git clone <your-repo>
cd your-app
npm install
npm run dev
```

## 文档

- **[开发者文档](docs/developer/)** - 架构、模式和详细指南
- **[用户指南](docs/userguide/)** - 最终用户文档模板
- **[使用此模板](docs/USING_THIS_TEMPLATE.md)** - 设置和工作流程指南

## 许可证

[MIT](LICENSE.md)

---

使用 [Tauri](https://tauri.app) | [shadcn/ui](https://ui.shadcn.com) | [React](https://react.dev) 构建
