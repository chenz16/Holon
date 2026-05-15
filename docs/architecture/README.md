# Holon Architecture

Status: draft
Date: 2026-05-15

Holon architecture is documented from three angles:

1. [`functional-architecture.md`](functional-architecture.md)
   Abstract product architecture: core concepts, data flow, workflow, state transitions, and system boundaries.

2. [`implementation-architecture.md`](implementation-architecture.md)
   Engineering architecture: how to implement the MVP with a local app, cloud connector, database, protocol layer, and Hermes runtime adapter.

3. [`ui-architecture.md`](ui-architecture.md)
   Product UI architecture: navigation, screen responsibilities, component model, visual system, and interaction rules for hybrid human-AI work.

The product principle is:

```text
Each local app can build a lightweight team.
Larger organization complexity comes from connected teams, not deep local agent hierarchy.
```

