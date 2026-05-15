# Implementation Architecture

Status: draft
Date: 2026-05-15

## Goal

This document defines how to build the Holon MVP.

Recommendation:

```text
MVP runtime base: Hermes
Product authority: Holon core
Cloud role: connector, relay, identity, reliability
Local role: team management, local execution, local context, mission inbox
```

Hermes should power local AI execution. Holon should own product state, permissions, missions, assignments, connections, and deliverables.

## High-Level Stack

Suggested MVP stack:

```text
apps/web
  Next.js app for local/hosted node UI and API routes

packages/core
  product domain logic: node, staff, assignments, missions, deliverables

packages/protocol
  peer/cloud protocol: dispatch, callback, ping, retry payloads

packages/runtime-hermes
  Hermes adapter: run local staff assignment and normalize events

packages/db
  schema, migrations, query helpers

packages/ui
  reusable UI components and design tokens

examples/two-node-demo
  local two-node replay script and seed data
```

## Deployment Modes

### Local-Only Node

Runs on a user's machine.

```text
local web app
local database
Hermes runtime adapter
optional cloud connector disabled
```

Use case: one person wants a local agent team without networking.

### Local Node With Cloud Connector

Runs locally but registers with Holon cloud.

```text
local web app
local runtime
local/private context
cloud relay for inbound/outbound missions
cloud identity and connection registry
```

Use case: personal local team connected to other people's teams.

### Hosted Node

Runs fully in cloud.

```text
hosted web app
managed database
managed runtime workers
cloud relay
```

Use case: company teams that do not want a local install.

MVP can start with local node plus direct peer protocol, then add cloud relay.

## Layer Responsibilities

### App Layer

Responsibilities:

- render UI
- expose node API
- handle owner interactions
- call core services
- stream status updates

Suggested routes:

```text
/today
/inbound
/staff
/connections
/deliverables
/settings
```

Suggested API routes:

```text
POST /api/assignments
GET  /api/assignments
GET  /api/assignments/:id
POST /api/missions
GET  /api/missions
POST /api/missions/:id/accept
POST /api/missions/:id/reject
POST /api/missions/:id/submit
POST /api/peer/done
GET  /api/peer/ping
POST /api/connections/test
POST /api/connections/rotate-token
```

### Core Domain Layer

The core layer owns business rules.

Modules:

```text
node-service
staff-service
assignment-service
mission-service
connection-service
deliverable-service
event-service
policy-service
router-service
```

The router chooses:

```text
target local AI staff -> runtime adapter
target proxy staff    -> protocol dispatch
target owner          -> local inbox/manual state
```

### Protocol Layer

The protocol layer serializes and validates network messages.

Message types:

- DispatchMission
- DispatchAccepted
- DispatchRejected
- CompletionCallback
- ConnectionPing
- TokenRotation
- ErrorResponse

It should provide:

- schema validation
- idempotency keys
- signing/token validation hooks
- protocol versioning
- retry metadata

MVP can use bearer-like tokens. Production should move toward scoped tokens with rotation and optional signed payloads.

### Runtime Adapter Layer

The runtime adapter converts a Holon assignment into an executable runtime job.

Contract:

```text
runAssignment(input) -> AsyncIterable<RuntimeEvent> + final DeliverableDraft
```

Input:

- assignment id
- staff id
- role/system instructions
- scoped context pack
- tool permissions
- budget
- timeout
- output expectations

Output:

- normalized events
- deliverable draft
- usage/cost metadata
- error state

### Hermes Adapter

Hermes is recommended for MVP because it already maps to the local-agent-team model:

- agent execution loop
- tools
- memory/skills concepts
- subagent or helper execution
- local runtime control

Holon should wrap Hermes instead of becoming Hermes.

Mapping:

| Holon Concept | Hermes Role |
|---|---|
| Local AI staff | configured Hermes agent profile |
| Assignment | bounded Hermes task |
| Temporary helper | Hermes subagent/helper |
| Tool policy | Hermes tool registry scope |
| Runtime events | normalized Holon events |
| Deliverable | final Holon artifact |

Do not let Hermes own:

- staff roster
- proxy connections
- missions
- assignment status
- deliverable records
- human approvals
- cloud identity

### Database Layer

Core tables:

```text
nodes
owners
staff
connections
assignments
missions
deliverables
events
runtime_jobs
retry_queue
context_packs
files
policies
```

Important fields:

```text
staff.mode: local_ai | proxy
assignments.route: local_runtime | proxy | owner
assignments.status
missions.status
connections.status
connections.last_seen_at
connections.revoked_at
events.entity_type
events.entity_id
events.kind
events.payload
retry_queue.next_attempt_at
retry_queue.attempt_count
```

MVP can use Postgres for local and hosted modes. SQLite can be evaluated later for packaged desktop/local-first distribution.

## Cloud Connector

The cloud connector should not be mandatory for the earliest demo, but the architecture should reserve a clean place for it.

Cloud responsibilities:

- user identity
- node registration
- connection registry
- mission relay when direct peer is unavailable
- retry durability
- token issuing and revocation
- audit log aggregation
- optional hosted node execution

Cloud should not require uploading all local context by default.

## Reliability

Required MVP reliability features:

- idempotency key on dispatch and callback
- retry queue for failed outbound dispatch
- heartbeat/ping
- visible failed state in UI
- timeout escalation
- token revoke path

Retry behavior:

```text
dispatch failed
  -> event: proxy_dispatch_failed
  -> retry_queue row created
  -> assignment status = retrying
  -> retry with exponential backoff
  -> after max attempts status = blocked
```

## Security

MVP:

- random inbound token per node
- connection-specific outbound token
- HTTPS required outside localhost
- revoke connection deletes or disables token
- rate limit by connection
- no broad local filesystem access for runtime

Production:

- scoped tokens
- token rotation
- signed protocol payloads
- per-action permissions
- organization policies
- audit export
- encrypted context packs

## Implementation Milestones

### M0: Repo And Schema

- app shell
- database schema
- migrations
- seed owner/node
- staff list

### M1: Local Runtime

- create local AI staff
- create assignment
- run stub runtime
- store deliverable
- replace stub with Hermes adapter

### M2: Proxy Protocol

- create proxy staff
- configure remote URL/token
- send mission
- receive mission
- return deliverable

### M3: Reliability

- retry queue
- ping
- connection status
- failure UI

### M4: Cloud Connector

- node registration
- cloud relay
- hosted connection registry
- cloud-managed tokens

## Engineering Rule

Keep the product state above the runtime.

```text
Holon decides what work exists, who owns it, who can receive it, and when it is done.
Hermes executes bounded local AI work.
The protocol moves missions and deliverables between nodes.
```

