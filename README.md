# AgentOS Workspace

AgentOS 是一组面向开发者和应用宿主的 AI 运行时工程，目标是把 LLM 调用、工具调用、本地模型、云端模型、应用注册、智能体会话和上下文能力整理成统一入口。

当前目录是 AgentOS 相关工程的聚合工作区。这里不是单一 monorepo：大部分子目录本身就是独立仓库，分别承担桌面端、Server 端、SDK、OpenTool、开发者文档和业务应用等职责。后续仍会有部分工程继续补入。

## 产品形态

AgentOS 目前按运行环境分为三类核心形态，并配套移动端客户端、官网、开发者文档和应用市场等分发入口：

| 形态 | 说明 | 典型入口 |
| --- | --- | --- |
| 桌面端版本 | 本地优先的桌面 AI OS。提供窗口管理、应用生态、模型管理、本地 LLM/ASR/Embedding、OpenTool 工具守护进程与 AgentOS API 网关。 | `agentos-desktop` |
| Server 版本 | 面向团队/企业部署的中心化服务。入口是 Web，提供用户、角色、应用、大模型、OAuth2、AgentKit/AppKit/LLMKit 等服务能力。 | `agentos-web` + `agentos-server` |
| Embedded 版本 | 面向草莓派或低成本小主机的本地端侧运行环境。重点是轻量网关、本地工具、本地模型和少量云端兜底。当前能力分散在 API、Kernel、OpenTool、Cactus 本地模型 SDK 等工程中。 | `agentos-api`、`agentos-kernel`、`opentool-util-daemon`、`cactus_openai` |
| 移动端客户端 | 面向 Android / iOS 的 AgentOS 客户端。可连接 AgentOS SDK，也可通过 Cactus 和内置模型做端侧推理与语音识别。 | `agentos_mobile` |

## 核心能力

AgentOS 对外提供的是一组 Kit 和 SDK，而不是只提供一个聊天界面。

| 能力 | 作用 | 相关工程 |
| --- | --- | --- |
| AgentKit | 智能体管理、会话初始化、流式对话、函数调用回调、Skills 传递。 | `agentos-api`、`agentos-server`、`liteagent_sdk_dart` |
| AppKit | 应用注册、bundle token、SSE 订阅、系统向应用发起 callApp。 | `agentos-api`、`agentos-server`、`agentos-sdk-*` |
| LLMKit / ModelKit | 统一模型列表、OpenAI 兼容 chat、embedding、ASR、TTS 等模型能力。 | `agentos-api`、`agentos-server`、`cactus_openai` |
| ToolKit / OpenTool | Agent 与工具之间的通用工具调用协议、工具服务端、客户端、守护进程和工具生命周期管理。 | `opentool*`、`agentos-sdk-ts/opentool` |
| Kernel | 记忆、偏好、RAG、Cron、外部服务健康监控、上下文检索。 | `agentos-kernel` |
| Auth / OAuth2 | 登录、Token、OAuth2/OIDC、企业身份接入。 | `agentos-auth`、`agentos-server-oauth2`、`agentos-server` |
| ChatKit | 可嵌入到 Flutter 或 Web/DOM 应用的 AgentOS 聊天侧边栏和会话控制器。 | `chatkit-dart`、`chatkit-agentos-ts` |

## 目录地图

### 运行时与服务端

| 目录 | 技术栈 | 角色 |
| --- | --- | --- |
| `agentos-api` | Dart / Shelf | 本地 AgentOS API 网关，默认 `http://127.0.0.1:8888`。暴露 AgentKit、AppKit、CronKit、RagKit、ToolKit、ModelKit/LLMKit。 |
| `agentos-kernel` | Dart | Service 级内核包。负责 memory、preference、RAG、Cron、LiteAgent/OpenTool/Embedding 健康状态。 |
| `agentos-server` | Java 21 / Spring Boot 3 / Sa-Token / MongoDB / Redis | Server 版本后端。包含 Root、App、AppKit、AgentKit、LLMKit、Model 管理、用户认证等模块。 |
| `agentos-server-oauth2` | Java 21 / Spring Boot 3 / Sa-Token | 独立 OAuth2/OIDC 授权服务器，支持客户端管理、JWT、钉钉扫码登录、Redis 会话。 |
| `agentos-web` | React / Vite / TypeScript / Ant Design | Server 版本 Web 入口。包含桌面工作台、系统管理、用户角色、App、大模型管理。 |

### 桌面端与应用宿主

| 目录 | 技术栈 | 角色 |
| --- | --- | --- |
| `agentos-desktop` | Flutter Desktop | AgentOS 桌面端主工程。包含桌面环境、窗口管理、模型管理、本地 AI 网关和内置应用生态。 |
| `agentos_mobile` | Flutter / Cactus | AgentOS 移动端客户端。支持 Android / iOS，内置模型 zip 通过脚本下载，首次运行解压到应用文档目录用于端侧推理和 ASR。 |
| `agentos_chat` | Flutter Desktop | LiteAgent / AgentOS LLM 桌面聊天客户端，支持流式回复、工具调用状态、会话持久化和托盘能力。 |
| `bret-browser` | Electron / React / TypeScript | 集成 AgentOS SDK 的 AI 浏览器，提供标签页、内部页面、侧边 Agent 和浏览器控制工具。 |
| `wynn-wiki` | Tauri / Web 前端 | 以 AgentOS 作为模型提供方的个人知识库应用，支持 LLM 摄入、知识图谱、Embedding 检索。 |
| `yoo-time` | Tauri / Rust / Web 前端 | AgentOS 增强的本地优先工作记录与复盘工具。通过本地记录和可选 AI 生成日报、问答和总结。 |
| `code-switch` | Tauri / Web 前端 | AI 编程 CLI 与模型配置管理工具，可作为 AgentOS 面向开发者生态的周边工具。 |

### SDK 与嵌入式组件

| 目录 | 语言 | 角色 |
| --- | --- | --- |
| `agentos-sdk-dart` | Dart | AgentOS Dart SDK，封装 Root、AgentKit、AppKit、CronKit、LlmKit、ToolKit。 |
| `agentos-sdk-ts` | TypeScript | AgentOS TypeScript SDK，支持浏览器/Node 场景，并显式导出 OpenTool/daemon 相关能力。 |
| `liteagent_sdk_dart` | Dart | LiteAgent Dart SDK，负责 LiteAgent 会话、流式对话、函数调用和 Skills。 |
| `agentos-auth` | Dart | AgentOS 用户认证 SDK，提供登录、刷新 Token、登出和本地 Token 持久化。 |
| `chatkit-dart` | Dart / Flutter | 可嵌入 Flutter 应用的 AgentOS 聊天侧边栏、Runtime 和 Session Controller。 |
| `chatkit-agentos-ts` | TypeScript / DOM | 可嵌入 Web/DOM 应用的 AgentOS 聊天侧边栏、Runtime 和 Session Controller。 |
| `cactus_openai` | Dart / Flutter FFI | 本地 Cactus 模型的 OpenAI 风格 SDK 适配层，面向端侧 chat 和 ASR。 |

### OpenTool 工具生态

| 目录 | 技术栈 | 角色 |
| --- | --- | --- |
| `opentool` | Spec + SDK | OpenTool 协议定义。专注 Agent 与 LLM 的工具调用，使用 JSON-RPC over HTTP。 |
| `opentool-dart` | Dart | OpenTool Dart SDK，包含 client、server 和 JSON 规范解析。 |
| `opentool-typescript` | TypeScript | OpenTool TypeScript SDK，包含 HTTP/JSON-RPC 客户端、服务端运行时和 JSON loader。 |
| `opentool-util-daemon` | Dart | 本地 OpenTool Daemon，管理工具构建产物、运行中 Tool 进程、API Key 和 SSE 生命周期事件。 |
| `opentool-daemon-client` | TypeScript | 访问本地 `opentoold` 的纯 TypeScript 客户端。 |
| `opentool-hub` | 待补充 | OpenTool Hub/Registry 相关工程占位。 |

### 官网、文档、应用市场与业务应用

| 目录 | 技术栈 | 角色 |
| --- | --- | --- |
| `agentos-homepage` | HTML / CSS / JavaScript | AgentOS Desktop 静态官网。包含 Hero、应用生态、内核能力、下载区和中英文切换。 |
| `agentos-dev-docs` | VitePress | 面向外部开发者的文档站，覆盖产品概览、Kit 说明、SDK 快速开始和集成指南。 |
| `agent-store-website` | Next.js / Spring Boot / MariaDB | Agent Store 网站和后端服务，提供公开 App 列表与详情 API。 |
| `agentos-dcc-app` | Java 21 / Spring Boot 3 / MongoDB / Redis | DCC 业务应用服务。包含文控、ECN、K3Cloud、BOM、文件工具、OpenTool 工具函数和 AgentKit 集成。 |

## 典型启动路径

### 本地桌面 / Embedded 网关

```bash
cd agentos-api
dart pub get
./tool/run_server.sh
```

默认服务地址：

```text
http://127.0.0.1:8888
```

### 桌面端

```bash
cd agentos-desktop
git lfs pull
flutter pub get
flutter run -d macos
```

Windows 和 Linux 分别使用 `-d windows`、`-d linux`。

### 移动端

```bash
cd agentos_mobile
bash scripts/fetch_builtin_model.sh
flutter pub get
flutter build apk --release
```

iOS 使用：

```bash
flutter build ios --release
```

内置模型不提交到 git。构建前需要先把 `assets/data/models_catalog.json` 中 `builtin` 和 `builtinAsr` 指向的 zip 下载到 `assets/models/`。

### Server 版本 Web 入口

```bash
cd agentos-web
npm install
cp .env.example .env
npm run dev
```

`agentos-web` 默认部署前缀是 `/agentos-web`，开发代理通过 `VITE_BASE_API` 指向后端。

### Java Server

```bash
cd agentos-server
cp .env.example .env
mvn clean install
```

运行入口位于：

```text
agentos-server/agent-os-server
```

需要按 `.env.example` 配置 Redis、MongoDB、OAuth2、LiteAgent Engine、前端地址和邮件服务。

### 开发者文档

```bash
cd agentos-dev-docs
npm install
npm run docs:dev
```

### 静态官网

```bash
cd agentos-homepage
python3 -m http.server 8000
```

然后访问：

```text
http://localhost:8000
```

该项目没有构建步骤，按静态站点发布 `index.html` 和 `assets/` 即可。

## 开发约定

- 顶层目录用于聚合与说明；各子工程保留自己的构建、测试、发布节奏。
- 新增工程时，优先补充本 README 的“目录地图”，再在对应工程内维护详细 README。
- 对外 API 事实以源码仓库和开发者文档为准：`agentos-dev-docs` 负责统一阅读路径，各源码仓库负责实现细节和可运行示例。
- Server 端与桌面端都应通过 SDK 或 Kit 边界交互，避免直接耦合具体 UI 或底层模型实现。
- Embedded 版本应优先复用 `agentos-api`、`agentos-kernel`、`opentool-util-daemon` 和端侧模型适配，而不是重新定义一套协议。

## 当前状态

这个工作区仍在快速整理中：

- 已存在的工程覆盖桌面端、移动端、Server 端、SDK、OpenTool、官网、文档站和若干 AgentOS 应用。
- Embedded 版本还不是一个单独目录，当前更像一组可裁剪组件的组合。
- `opentool-hub` 和部分后续工程仍待补充。
- 各子工程内已有 README 的，以子工程 README 为详细开发说明。
