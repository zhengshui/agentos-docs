# Kit 架构

本文描述 AgentOS 的核心 Kit 视角，帮助新功能在正确的能力边界内演进。

## Kit 总览

| Kit | 职责 | 典型接口或能力 | 相关工程 |
| --- | --- | --- | --- |
| AgentKit | 智能体、会话、流式对话、函数调用回调、Skills 传递。 | agent session、chat stream、tool call callback | `agentos-api`、`agentos-server`、`liteagent_sdk_dart` |
| AppKit | 应用注册、bundle token、SSE 订阅、宿主向应用发起 callApp。 | app register、app events、callApp | `agentos-api`、`agentos-server`、`agentos-sdk-*` |
| LLMKit / ModelKit | 模型列表、OpenAI 兼容 chat、embedding、ASR、TTS。 | chat completions、embeddings、models | `agentos-api`、`agentos-server`、`cactus_openai` |
| ToolKit / OpenTool | Agent 与工具之间的通用工具调用协议和运行时。 | JSON-RPC over HTTP、tool server、daemon | `opentool*`、`opentool-util-daemon` |
| Kernel | 记忆、偏好、RAG、Cron、健康检查和上下文检索。 | memory、preference、rag、cron、health | `agentos-kernel` |
| Auth / OAuth2 | 登录、Token、OAuth2/OIDC、企业身份接入。 | OAuth2、OIDC、JWT、session | `agentos-auth`、`agentos-server-oauth2` |
| ChatKit | 可嵌入聊天 UI、Runtime 和会话控制器。 | sidebar、runtime、session controller | `chatkit-dart`、`chatkit-agentos-ts` |

## 边界规则

- 新能力如果会被多个 App 使用，应优先沉淀为 Kit 或 SDK 能力。
- UI 组件不应直接定义跨 Runtime 协议。
- 模型供应商差异应收敛在 LLMKit / ModelKit，而不是散落在 App 内。
- 工具调用协议应优先走 OpenTool，而不是为单个应用新增私有协议。
- 业务 App 可以持有业务模型，但不应绕过 AgentOS 的 Kit 边界访问底层工具和模型。

