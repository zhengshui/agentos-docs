# Web App 架构

本文维护 AgentOS 多 Web App 的架构视角。它用于判断一个新 Web App 是 Server 产品的一部分、开发者生态入口，还是 App 生态中的独立应用。

## Web App 分类

| 类型 | 说明 | 代表工程 |
| --- | --- | --- |
| Server Console | AgentOS Server 的管理和工作台入口。 | `agentos-web` |
| Docs Site | 面向开发者的文档站。 | `agentos-dev-docs` |
| Website | 官网、营销页、下载页。 | `agentos-homepage` |
| App Store | 应用市场、应用详情和发布流程。 | `agent-store-website` |
| Embedded Web UI | 某些本地/端侧运行时的轻量管理界面。 | 待规划 |
| Business App | 基于 AgentOS 能力的业务应用。 | `agentos-dcc-app` |

## 新 Web App 设计检查项

- 它属于哪类 Web App？
- owning repo 是独立工程还是现有工程内的模块？
- 是否需要登录、OAuth2、角色权限或租户边界？
- 是否依赖 AgentKit、AppKit、LLMKit、ToolKit 或 Kernel？
- 是否需要共享 UI 组件、SDK、OpenAPI 类型或设计系统？
- 是否需要进入 `agentos-dev-docs` 的开发者阅读路径？

