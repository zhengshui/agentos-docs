# ADR 0001: 使用根 docs 作为产品架构中枢

## 状态

Accepted

## 背景

AgentOS 工作区包含 20+ 个独立项目。它们覆盖 Desktop、Server、Mobile、SDK、OpenTool、文档站、官网、应用市场和业务应用。根目录不是单一 monorepo，大多数子目录由各自 Git 仓库管理。

随着多个 Web App 和应用继续增加，根文档如果按项目复制细节，会快速变成过期的 README 镜像；如果只记录临时任务，又无法帮助 AI Agent 理解长期产品结构。

## 决策

根 `docs` 定位为产品架构中枢，维护跨项目的产品结构、架构边界、能力矩阵、术语和长期决策。

单个子项目的安装、构建、测试、实现细节和短期 issue，继续维护在子项目内。

## 影响

- 根文档优先按 `product/`、`architecture/`、`context/`、`adr/`、`superpowers/` 分层。
- 新 App 或新 Web App 需要先在产品线、应用组合和能力矩阵中定位。
- 影响两个以上工程的改动，应先写 spec，再写 plan。
- AI Agent 进入工作区时，可以通过根文档快速建立产品和架构上下文。

