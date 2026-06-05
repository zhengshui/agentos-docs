# Superpowers 工作流

本目录保存 AI-native 开发过程中的方案设计、实施计划、复盘和交接材料。

根 `docs` 的长期产品和架构事实应放在 `product/`、`architecture/`、`context/` 和 `adr/` 中；`superpowers/` 只保存一次具体工作流产生的 spec、plan、review 或 handoff。

## 目录

| 目录 | 作用 |
| --- | --- |
| `specs/` | 设计规格。回答做什么、为什么、边界是什么。 |
| `plans/` | 实施计划。回答怎么拆任务、怎么验证、怎么交付。 |
| `reviews/` | 方案复盘、实现复盘或评审摘要。 |
| `handoffs/` | 多 Agent 或跨会话交接材料。 |

## 命名

```text
specs/YYYY-MM/YYYY-MM-DD-topic-slug-design.md
plans/YYYY-MM/YYYY-MM-DD-topic-slug-plan.md
```

## 规则

- spec 先于 plan。
- plan 必须指向已确认的 spec 或清晰的问题定义。
- 跨两个以上子项目的改动必须有 spec。
- 完成后如果形成长期事实，应同步更新根 `product/`、`architecture/`、`context/` 或 `adr/`。
