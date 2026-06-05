# 项目地图

本文是根 `README.md` 目录地图的协作版，用于帮助 AI Agent 和开发者快速定位 owning project。更完整的产品描述仍以根 `README.md` 为准。

## 运行时与服务端

| 项目 | 职责 |
| --- | --- |
| `agentos-api` | 本地 AgentOS API 网关。 |
| `agentos-kernel` | 服务级 Kernel 能力。 |
| `agentos-server` | Server 版本后端。 |
| `agentos-server-oauth2` | OAuth2/OIDC 授权服务器。 |
| `agentos-web` | Server 版本 Web 入口。 |

## 桌面端与应用宿主

| 项目 | 职责 |
| --- | --- |
| `agentos-desktop` | 桌面端主工程。 |
| `agentos_mobile` | 移动端客户端。 |
| `agentos_chat` | 桌面聊天客户端。 |
| `bret-browser` | AgentOS-enabled 浏览器。 |
| `wynn-wiki` | 个人知识库应用。 |
| `yoo-time` | 本地优先工作记录与复盘工具。 |
| `code-switch` | AI 编程 CLI 和模型配置工具。 |

## SDK 与组件

| 项目 | 职责 |
| --- | --- |
| `agentos-sdk-dart` | AgentOS Dart SDK。 |
| `agentos-sdk-ts` | AgentOS TypeScript SDK。 |
| `liteagent_sdk_dart` | LiteAgent Dart SDK。 |
| `agentos-auth` | 用户认证 SDK。 |
| `chatkit-dart` | Flutter ChatKit。 |
| `chatkit-agentos-ts` | Web/DOM ChatKit。 |
| `cactus_openai` | Cactus 本地模型 OpenAI 风格适配。 |

## OpenTool

| 项目 | 职责 |
| --- | --- |
| `opentool` | OpenTool 协议定义。 |
| `opentool-dart` | OpenTool Dart SDK。 |
| `opentool-typescript` | OpenTool TypeScript SDK。 |
| `opentool-util-daemon` | 本地 OpenTool Daemon。 |
| `opentool-daemon-client` | TypeScript daemon client。 |
| `opentool-hub` | OpenTool Hub/Registry 相关工程，当前细节待在子项目内补充。 |

## 官网、文档和业务应用

| 项目 | 职责 |
| --- | --- |
| `agentos-homepage` | AgentOS Desktop 静态官网。 |
| `agentos-dev-docs` | 开发者文档站。 |
| `agent-store-website` | Agent Store 网站和后端服务。 |
| `agentos-dcc-app` | DCC 业务应用服务。 |
