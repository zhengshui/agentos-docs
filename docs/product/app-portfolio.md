# 应用组合

本文维护 AgentOS 相关应用、Web App、站点和业务应用的组合视图。它不替代单个 App 的 README，而是回答这些应用在 AgentOS 产品体系中的位置。

## 字段说明

| 字段 | 含义 |
| --- | --- |
| App | 应用或站点名称。 |
| 类型 | Desktop App、Web App、Mobile App、Website、Business App、Tooling 等。 |
| 产品线 | 归属的产品线。 |
| owning repo | 主要负责工程。 |
| 目标用户 | 主要使用者。 |
| 依赖能力 | 依赖哪些 Kit、SDK、Runtime 或外部服务。 |
| 状态 | idea、active、maintenance、deprecated。 |

## 当前应用组合

| App | 类型 | 产品线 | owning repo | 目标用户 | 依赖能力 | 状态 |
| --- | --- | --- | --- | --- | --- | --- |
| AgentOS Desktop | Desktop App | Desktop | `agentos-desktop` | 本地 AI OS 用户 | AgentKit、AppKit、ModelKit、ToolKit、Kernel | active |
| AgentOS Web | Web App | Server | `agentos-web` | 团队/企业管理员和用户 | Server API、Auth、AgentKit、AppKit、LLMKit | active |
| AgentOS Mobile | Mobile App | Mobile | `agentos_mobile` | 移动端用户 | AgentOS SDK、Cactus、本地模型 | active |
| AgentOS Homepage | Website | Developer Ecosystem | `agentos-homepage` | 外部访客和潜在用户 | 静态站点 | active |
| AgentOS Dev Docs | Docs Site | Developer Ecosystem | `agentos-dev-docs` | 开发者 | VitePress、SDK 文档 | active |
| Agent Store | Website / Service | App Ecosystem | `agent-store-website` | App 发布者和使用者 | AppKit、后端服务 | active |
| Bret Browser | Desktop App | App Ecosystem | `bret-browser` | 浏览器用户、AgentOS 用户 | AgentOS SDK、浏览器工具 | active |
| Wynn Wiki | Desktop/Web App | App Ecosystem | `wynn-wiki` | 个人知识库用户 | LLMKit、Embedding、RAG | active |
| Yoo Time | Desktop/Web App | App Ecosystem | `yoo-time` | 本地优先工作记录用户 | LLMKit、本地数据 | active |
| Code Switch | Desktop/Web App | Developer Ecosystem | `code-switch` | AI 编程用户 | CLI 配置、模型配置 | active |
| DCC App | Business App | App Ecosystem | `agentos-dcc-app` | 业务系统用户 | AgentKit、OpenTool、业务后端 | active |

## 新增 App 检查项

- 明确产品线和 owning repo。
- 明确是否需要独立 Git 仓库。
- 明确依赖哪些 Kit 或 SDK。
- 明确是否需要进入 Agent Store 或开发者文档。
- 明确根文档只记录组合视图，详细实现留在子项目。

