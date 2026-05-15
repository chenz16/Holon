<p align="center">
  <img src="docs/assets/holon-logo.svg" alt="Holon logo: a fractal network of local human-AI teams" width="760">
</p>

# Holon

Local human-AI teams that connect into a larger hybrid workforce network.

Holon is a commercial application and framework for hybrid real and virtual workforces. Each local app is a lightweight team desk: a real person can create and manage a small team of local AI agents, proxy people, inbound missions, outbound assignments, and returned deliverables. Complexity grows through connected desks, not through deep local agent hierarchies.

## 中文解释

Holon 的核心不是做一个无限复杂的 AI 员工组织图，而是让每个人都拥有一个轻量的本地团队。这个本地团队可以独立工作，也可以连接到其他真人和他们的本地团队。

这个名字来自 **holon** 的系统论概念：一个单元既是完整的整体，也是更大系统的一部分。用在 Holon 里就是：

- 每个 local app 都是一个完整的小团队
- 每个小团队又是更大职场网络里的一个节点
- 虚拟员工可以连接真人，真人也可以把任务交给自己的虚拟团队
- 复杂度通过团队之间的连接和级联产生，而不是通过单个 app 内部无限加深 agent 层级

Logo 采用分形式结构：中心节点代表一个本地团队，周围的小节点代表本地 AI 员工、proxy 真人和远端团队；每个小节点继续分叉，表示每个团队都可以成为下一级完整团队。它表达的是“局部像整体，整体由局部级联而成”。

## Core Idea

Every Holon node is both:

- a complete local team that can work on its own
- a node in a larger network of human and AI teams

This follows the holon principle: each unit is a whole and a part at the same time.

```text
Person / team owner
  -> local lightweight agent team
      -> local AI staff
      -> proxy staff connected to real people
      -> inbound missions from other nodes
      -> outbound assignments to other nodes
```

## Product Direction

Holon is not just an AI employee dashboard. It is a work network where:

- real people can run their own local agent teams
- virtual staff can act as proxies for real people
- remote people can accept, reject, execute, or forward missions
- work can cascade across teams while accountability stays human-owned
- cloud services provide connection, routing, identity, policy, and reliability
- local apps preserve each user's own tools, context, and operating style

## Repository Status

This repository is the clean commercial restart of the earlier Mibusy V3 prototype.

The initial design is documented in:

- [`docs/product/holon-product-definition.md`](docs/product/holon-product-definition.md)
- [`docs/product/mvp-scope.md`](docs/product/mvp-scope.md)
