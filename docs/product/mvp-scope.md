# Holon MVP Scope

Status: draft
Date: 2026-05-15

## Goal

Build the smallest commercial-grade Holon app that proves the core loop:

```text
local team -> proxy staff -> remote human/team -> returned deliverable
```

The MVP should not reproduce the full Mibusy prototype. It should extract the essential human-agent interconnect workflow into a clean app and framework.

## MVP Boundary

MVP should only include agent execution that Holon can fully control.

Included:

- local app owned by the user
- local AI staff executed through Holon's runtime adapter
- Hermes-based agent execution for local staff
- Holon API-managed assignments, missions, handoffs, and deliverables
- direct peer/cloud dispatch between Holon nodes

Not included in MVP:

- taking over external agent CLI windows such as Codex CLI, Claude Code CLI, or other giant-provider agent terminals
- parallel integration with Cowork-like external agent products
- treating third-party agent results as first-class remote workers
- native mobile app runtime
- desktop terminal takeover as a human control surface

These cases should be considered in architecture, but not implemented in the first MVP. MVP should prove the core loop with agents Holon owns and controls.

## MVP User Story

As a team owner, I can:

1. create a local AI staff member
2. create a proxy staff member connected to another Holon node
3. assign work to either staff type
4. receive inbound missions from other nodes
5. accept or reject inbound work
6. complete work manually or with local AI staff
7. submit a deliverable back to the sender
8. see the returned deliverable attached to the original assignment

## Must Have

### Local Node

- owner profile
- local team roster
- local assignments
- inbound mission inbox
- deliverables
- local database
- local runtime adapter interface

### Staff

- create local AI staff
- create proxy staff
- edit name, role, and description
- archive staff
- show mode: local AI or proxy
- show current status

### Connections

- generate inbound token
- configure proxy staff with remote URL and token
- test connection
- revoke connection
- show last seen

### Assignments

- create assignment
- route to local AI staff
- route to proxy staff
- show status
- show routing metadata
- show retry/failure states

### Missions

- receive inbound mission
- list pending missions
- accept mission
- reject mission
- submit deliverable
- call back to origin node

### Deliverables

- create deliverable on local AI completion
- create deliverable on remote completion callback
- attach deliverable to source assignment
- list returned deliverables

### Demo

- two-node local demo
- scripted setup
- scripted end-to-end verification

## Should Have

- retry queue for failed peer dispatch
- heartbeat endpoint
- online/offline status pill
- token rotation
- per-connection task limit
- simple audit event log
- basic cloud relay abstraction

## Defer

- native mobile shell
- phone-native local runtime
- desktop CLI takeover for real-person work sessions
- external giant-provider agent CLI orchestration
- Cowork-like external agent result ingestion as first-class workers
- marketplace
- public discovery
- workspace billing
- multi-hop graph UI
- complex permissions
- revenue sharing
- end-to-end encrypted payloads
- deep integration marketplace

## Suggested Repo Structure

```text
apps/web
packages/core
packages/protocol
packages/runtime-hermes
packages/db
packages/ui
examples/two-node-demo
docs/product
docs/architecture
```

## First Engineering Milestones

### M0: Project Shell

- Next.js app or equivalent web shell
- database schema
- local auth placeholder
- owner profile
- basic staff list

### M1: Local Team

- create local AI staff
- assignment model
- deliverable model
- runtime adapter interface
- stub local runner

### M2: Proxy Staff

- proxy staff mode
- connection token model
- remote URL/token config
- connection test endpoint

### M3: Mission Protocol

- inbound mission endpoint
- outbound dispatch
- completion callback
- mission inbox UI
- returned deliverable UI

### M4: Reliability

- retry queue
- heartbeat
- visible failure states
- audit events

### M5: Cloud Connector

- cloud relay API
- hosted connection registry
- local app registration
- basic policy controls

## Acceptance Criteria

The MVP is acceptable when:

1. a new user understands that each local app can build a small team
2. proxy staff are visibly connected to real people or remote teams
3. a task can be sent from node A to node B
4. node B can accept and submit work
5. node A receives the deliverable under the original assignment
6. offline or failed peer dispatch is visible
7. the demo can be replayed from a clean checkout
