# 根 docs 产品架构中枢设计

## 背景

AgentOS 当前工作区是多项目聚合目录，包含 20+ 个独立 Git 仓库。根目录需要支持多项目调度、新功能设计、文档整理和后续多 Agent 协作。

用户明确选择根 `docs` 优先作为产品架构中枢，而不是调度中枢。

## 目标

- 让 AI Agent 能快速理解 AgentOS 的产品线、架构边界和能力分布。
- 避免根文档复制子项目 README 的实现细节。
- 为后续多个 Web App 和 AgentOS 应用提供统一的产品归属和架构定位。
- 让长期决策从聊天中沉淀到可提交的 Git 文档。

## 非目标

- 不在根 `docs` 维护每个子项目的完整开发手册。
- 不把根仓库改造成 monorepo。
- 不用根文档替代子项目 issue、release note 或测试说明。

## 文档结构

```text
docs/
  README.md
  product/
  architecture/
  context/
  adr/
  superpowers/
```

## 分层职责

| 分层 | 职责 |
| --- | --- |
| `product/` | 产品线、应用组合、路线图和使用场景。 |
| `architecture/` | 系统总览、Kit 架构、运行时拓扑、集成边界和 Web App 架构。 |
| `context/` | 项目地图、能力矩阵、术语和当前状态。 |
| `adr/` | 长期产品和架构决策。 |
| `superpowers/` | 具体设计和实施计划产物。 |

## Agent 使用方式

AI Agent 进入工作区时，优先阅读：

1. 根 `README.md`
2. `docs/README.md`
3. `docs/context/project-map.md`
4. `docs/context/capability-matrix.md`
5. 与当前任务相关的 `product/` 或 `architecture/` 文件

跨两个以上子项目的改动，应先写 spec，再写 plan。实现完成后，如果产生长期事实，应更新产品、架构或上下文文档。

## 验收标准

- 根 `docs` 有清晰入口。
- 产品线、应用组合、系统总览、Kit 架构和能力矩阵都有初始文档。
- 有 ADR 记录根 `docs` 的定位。
- `superpowers` 目录保留为工作流产物区，而不是长期事实区。

