# 系统总览

AgentOS 是一组围绕 AI Runtime、Kit、SDK、工具协议和应用宿主组织的工程集合。根工作区用于协调这些工程的产品和架构关系，而不是把它们合并成单一 monorepo。

## 高层结构

```text
User / Developer
  -> Apps / Web Apps / Desktop Shell / Mobile Client
  -> SDKs and ChatKit
  -> AgentKit / AppKit / LLMKit / ToolKit / Kernel / Auth
  -> Local Runtime, Server Runtime, Embedded Runtime
  -> Models, Tools, Memory, RAG, Cron, External Services
```

## 主要层级

| 层级 | 作用 | 代表工程 |
| --- | --- | --- |
| App 层 | 承载用户体验和具体业务场景。 | `agentos-desktop`、`agentos-web`、`bret-browser`、`wynn-wiki` |
| SDK / Embeddable 层 | 为 App 接入 AgentOS 能力提供稳定接口。 | `agentos-sdk-dart`、`agentos-sdk-ts`、`chatkit-dart`、`chatkit-agentos-ts` |
| Kit 层 | 定义 Agent、App、LLM、Tool、Kernel、Auth 等能力边界。 | `agentos-api`、`agentos-server`、`agentos-kernel`、`opentool*` |
| Runtime 层 | 提供本地、服务端、端侧或嵌入式运行环境。 | `agentos-desktop`、`agentos-server`、`agentos-api`、`agentos_mobile` |
| Infrastructure 层 | 模型、数据库、缓存、工具进程和外部服务。 | Cactus、本地模型、MongoDB、Redis、OpenTool Daemon |

## 架构原则

- App 不直接耦合底层模型或工具进程，优先通过 Kit 和 SDK 接入。
- Desktop、Server、Embedded、Mobile 可以共享 Kit 语言，但允许 Runtime 实现不同。
- OpenTool 是 Agent 和工具生态之间的协议边界。
- Kernel 能力应服务于记忆、偏好、RAG、Cron、健康检查和上下文，不承担 UI 职责。
- 根文档记录跨项目结构；项目内部实现以子项目源码和 README 为准。

