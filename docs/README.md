# AgentOS Docs

本目录是 AgentOS 聚合工作区的产品架构中枢，用于维护跨项目的产品结构、架构边界、能力分布和长期决策。

这里不是各子项目 README 的镜像。单个工程的安装、构建、测试和实现细节，应优先维护在对应子项目内；根 `docs` 只记录跨项目协作时必须共享的事实和决策。

## 文档分层

| 目录 | 作用 | 典型问题 |
| --- | --- | --- |
| `product/` | 产品线、应用组合、路线图和使用场景。 | AgentOS 要成为什么产品？哪些 App 属于哪条产品线？ |
| `architecture/` | 跨项目架构、Kit 边界、运行时拓扑和 Web App 架构。 | 能力如何落到 Runtime、Kit、SDK 和 App？ |
| `context/` | 当前项目地图、能力矩阵、术语和状态。 | 20+ 工程各自负责什么？当前能力分布在哪里？ |
| `adr/` | 架构和产品方向的重要决策记录。 | 为什么这样组织工程、文档或技术边界？ |
| `superpowers/` | AI-native 方案设计和实施计划产物。 | 某个新功能或架构调整的 spec/plan 是什么？ |

## 使用规则

- 根文档优先回答跨项目问题，不替代子项目文档。
- 新增顶层项目时，先更新根 `README.md` 的目录地图，再按需要更新 `context/project-map.md` 和 `context/capability-matrix.md`。
- 新增产品线、App 或 Web App 时，更新 `product/app-portfolio.md`，再补充必要的架构边界。
- 形成长期架构决策时，新增 `adr/NNNN-*.md`。
- AI Agent 进入本工作区时，优先阅读根 `README.md`、本文件、`context/project-map.md` 和 `context/capability-matrix.md`。

