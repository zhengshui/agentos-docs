# 集成边界

本文记录跨项目集成时应遵守的边界。目标是减少直接耦合，让多个 App、SDK 和 Runtime 能稳定演进。

## 推荐边界

| 调用方 | 被调用方 | 推荐方式 | 避免方式 |
| --- | --- | --- | --- |
| Web App | Server Runtime | HTTP API / SDK | 直接依赖后端内部模块 |
| Desktop App | Local Runtime | `agentos-api` / SDK / Kit API | 直接操作工具进程或模型文件 |
| App | Tool | OpenTool / ToolKit | 私有临时 RPC 协议 |
| UI | Model Provider | LLMKit / ModelKit | 在 UI 内散落 provider 适配 |
| Business App | AgentOS | AgentKit / AppKit / SDK | 绕过 Kit 调用底层实现 |
| Docs Site | Source Projects | 稳定 API 和示例 | 复制大量内部实现细节 |

## 判断标准

- 如果接口会被多个项目复用，应进入 SDK 或 Kit。
- 如果接口只服务单个业务 App，可以先留在该 App 内，但要记录是否有抽象为 Kit 的可能。
- 如果跨项目依赖需要同步版本，应明确 release 或兼容策略。
- 如果一个变更会影响两个以上工程，应先写 spec，再写 plan。

