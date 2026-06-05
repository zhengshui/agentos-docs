# 产品线

本文维护 AgentOS 的产品线划分。它用于帮助人和 AI Agent 判断一个新功能、新 App 或新 SDK 应该归属到哪个产品方向。

## 当前产品线

| 产品线 | 目标 | 主要入口 | 相关工程 |
| --- | --- | --- | --- |
| Desktop | 本地优先的桌面 AI OS，承载窗口、应用、模型、本地工具和 Agent 会话。 | `agentos-desktop` | `agentos-desktop`、`agentos-api`、`agentos-kernel`、`opentool-util-daemon` |
| Server | 面向团队/企业的中心化 AgentOS 服务。 | `agentos-web` + `agentos-server` | `agentos-web`、`agentos-server`、`agentos-server-oauth2` |
| Embedded | 面向低成本小主机和端侧设备的轻量运行环境。 | 待形成独立入口 | `agentos-api`、`agentos-kernel`、`opentool-util-daemon`、`cactus_openai` |
| Mobile | Android / iOS 客户端和端侧模型体验。 | `agentos_mobile` | `agentos_mobile`、`cactus_openai`、`agentos-sdk-dart` |
| Developer Ecosystem | 面向开发者的 SDK、OpenTool、文档站、官网和工具。 | SDK / Docs / Website | `agentos-sdk-*`、`opentool*`、`agentos-dev-docs`、`agentos-homepage` |
| App Ecosystem | 可被 AgentOS 宿主、注册、分发或增强的应用。 | App Store / 独立 App | `agent-store-website`、`bret-browser`、`wynn-wiki`、`yoo-time`、`code-switch` |

## 归属规则

- 如果功能影响运行时能力，先判断属于 Desktop、Server、Embedded 还是 Mobile。
- 如果功能影响 Agent、App、LLM、Tool、Kernel、Auth 或 Chat 这类通用能力，优先归入 Kit/SDK 视角。
- 如果功能只影响单个应用体验，产品归属记录在 `app-portfolio.md`，实现细节留在子项目。
- 如果一个能力跨多条产品线，必须在 `architecture/integration-boundaries.md` 说明边界。

