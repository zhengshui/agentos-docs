# 运行时拓扑

本文维护 AgentOS 在不同运行环境中的拓扑差异，避免 Desktop、Server、Embedded 和 Mobile 在实现时混淆边界。

## Desktop

```text
agentos-desktop
  -> agentos-api
  -> agentos-kernel
  -> opentool-util-daemon
  -> local / cloud models
```

Desktop 重点是本地优先、窗口宿主、应用生态、本地模型和本地工具进程。

## Server

```text
agentos-web
  -> agentos-server
  -> agentos-server-oauth2
  -> MongoDB / Redis / external model providers
```

Server 重点是团队/企业场景、用户角色、中心化配置、OAuth2/OIDC 和集中式 AgentOS 服务。

## Embedded

```text
embedded host
  -> agentos-api
  -> agentos-kernel
  -> opentool-util-daemon
  -> cactus_openai / lightweight models
```

Embedded 当前不是独立顶层工程，更像可裁剪组件组合。新设计应优先复用现有 API、Kernel、OpenTool 和 Cactus 能力。

## Mobile

```text
agentos_mobile
  -> agentos-sdk-dart
  -> cactus_openai
  -> optional AgentOS server / local runtime
```

Mobile 重点是端侧体验、模型资源管理、移动端交互和可选云端兜底。

