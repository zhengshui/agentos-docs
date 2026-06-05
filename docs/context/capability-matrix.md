# 能力矩阵

本文从能力视角映射 AgentOS 的工程分布。它用于判断新需求应该落在哪个 Kit、SDK、Runtime 或 App。

| 能力 | 主要工程 | 支撑工程 | 说明 |
| --- | --- | --- | --- |
| Agent 会话 | `agentos-api`、`agentos-server` | `liteagent_sdk_dart`、`agentos-sdk-*` | 智能体管理、会话初始化、流式对话和工具调用回调。 |
| App 注册与宿主 | `agentos-api`、`agentos-server` | `agentos-sdk-*` | AppKit、bundle token、SSE、callApp。 |
| 模型调用 | `agentos-api`、`agentos-server` | `cactus_openai` | OpenAI 兼容 chat、embedding、ASR、TTS 和模型列表。 |
| 工具调用 | `opentool`、`opentool-util-daemon` | `opentool-dart`、`opentool-typescript` | OpenTool 协议、工具服务端、客户端和守护进程。 |
| Kernel | `agentos-kernel` | `agentos-api` | 记忆、偏好、RAG、Cron、健康检查和上下文。 |
| 认证授权 | `agentos-server-oauth2`、`agentos-auth` | `agentos-server` | 登录、Token、OAuth2/OIDC、企业身份接入。 |
| Web 管理台 | `agentos-web` | `agentos-server` | Server 版本 Web 入口。 |
| 桌面宿主 | `agentos-desktop` | `agentos-api`、`opentool-util-daemon` | 桌面环境、窗口管理、本地模型和工具生态。 |
| 移动端 | `agentos_mobile` | `cactus_openai`、`agentos-sdk-dart` | 移动端 AgentOS 客户端和端侧模型体验。 |
| 开发者文档 | `agentos-dev-docs` | 根 `docs`、各 SDK | 对外开发者阅读路径。 |
| 官网与分发 | `agentos-homepage`、`agent-store-website` | `agentos-dev-docs` | 官网、下载、应用市场和应用详情。 |

## 使用方式

- 新需求先在本表中定位能力，再进入对应 owning project。
- 如果一个能力横跨多个工程，先写 spec 明确边界。
- 如果一个 App 需要新通用能力，优先评估是否应进入 Kit 或 SDK。

