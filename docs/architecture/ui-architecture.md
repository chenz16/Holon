# UI Architecture

Status: draft
Date: 2026-05-15

## Purpose

This document defines how Holon's product UI should be designed.

The UI must make the product model obvious:

```text
each local app is a small team
each team can connect to other real people and their teams
work moves through visible missions, assignments, and deliverables
```

## UX Position

Holon is not an agent graph editor.

The primary user is an operator, founder, manager, or team lead. They need to know:

- what is happening now
- what needs their decision
- who or what is doing the work
- which work is local
- which work is remote through proxy staff
- what came back as a deliverable
- which connections are healthy or failing

## Information Architecture

Primary navigation:

```text
Today
Inbound
Staff
Connections
Deliverables
```

Secondary navigation:

```text
Settings
Context
Policies
Audit
```

MVP should focus on the primary five.

## Screen Responsibilities

### Today

Purpose: the owner's live work cockpit.

Show:

- active assignments
- waiting remote work
- returned deliverables
- blocked/retrying work
- inbound mission alerts
- quick assign action

Assignment cards must show route:

```text
Local AI
Proxy human
Manual owner
Waiting remote
Retrying
Blocked
Returned
```

Do not hide routing behind detail screens. Hybrid work only makes sense if the user can see where work is.

### Inbound

Purpose: receive work from other people or teams.

Mission card fields:

- sender
- sender node/team
- requested outcome
- context summary
- urgency/deadline
- current status
- callback target

Actions:

- accept
- reject
- ask question
- delegate locally
- submit deliverable

Accepted missions should be able to create local assignments.

### Staff

Purpose: manage the local team.

Group staff by mode:

```text
Local AI
Proxy People
System Defaults
Archived
```

Staff card fields:

- name
- role
- mode
- current status
- current assignment
- last deliverable
- budget/limit
- connection status if proxy

Proxy staff must be visually distinct from local AI staff.

Example:

```text
小王
Media Research Proxy
Runs through: Wang's Desk
Status: healthy, last seen 2m ago
```

### Connections

Purpose: manage network trust and reliability.

Show:

- inbound token
- outbound proxy connections
- remote label
- URL/relay target
- status
- last seen
- last successful dispatch
- permissions
- revoke/rotate/test actions

Connection states should use plain operational language:

```text
Healthy
Offline
Retrying
Invalid token
Revoked
```

### Deliverables

Purpose: preserve returned work as artifacts.

Groups:

- local AI deliverables
- remote proxy deliverables
- deliverables submitted upstream

Deliverable card fields:

- title
- source assignment or mission
- author/staff
- local or remote
- created time
- status

Deliverables should link back to their assignment/mission timeline.

## Key User Flows

### Create Local AI Staff

```text
Staff
  -> Add staff
  -> Local AI
  -> name, role, instructions, budget
  -> save
  -> appears under Local AI
```

### Create Proxy Staff

```text
Staff
  -> Add staff
  -> Proxy person/team
  -> name, role, remote URL/token or cloud connection
  -> test connection
  -> save
  -> appears under Proxy People
```

### Assign Work

```text
Today
  -> New assignment
  -> describe outcome
  -> choose target staff or auto route
  -> confirm
  -> assignment appears with route badge
```

If target is proxy staff:

```text
assignment status = waiting_remote
remote mission created
connection event logged
```

### Accept Inbound Mission

```text
Inbound
  -> mission detail
  -> accept
  -> choose: do manually / assign to local staff / forward to proxy
  -> work starts
```

### Submit Deliverable

```text
mission detail
  -> submit deliverable
  -> preview
  -> send upstream
  -> mission status = submitted
  -> outbound event logged
```

## Component Model

Core components:

```text
AppShell
PrimaryNav
NodeStatusBar
AssignmentCard
MissionCard
StaffCard
ConnectionCard
DeliverableCard
RouteBadge
StatusBadge
Timeline
ActionDrawer
SubmitDeliverableDialog
ConnectionTestPanel
```

### RouteBadge

RouteBadge should make routing instantly clear.

Values:

```text
Local AI
Proxy
Inbound
Manual
Cloud Relay
```

### Timeline

Every assignment and mission detail should include a timeline.

Events:

- created
- routed
- dispatched
- received
- accepted
- runtime started
- deliverable submitted
- callback received
- completed
- failed

## Visual System

Holon should feel like a serious work product, not a chatbot toy.

Suggested direction:

- warm neutral base
- clear dark text
- restrained accent colors
- route/status badges for scanning
- compact cards
- no decorative gradient blobs
- no marketing hero as the app home

Suggested palette:

```text
paper: #F8F6EF
surface: #FFFFFF
line: #E4DFD2
ink: #1A1A18
muted: #6F6C63
gold: #C69A35
green: #2E7D52
blue: #1F6F9E
violet: #7B4FAB
red: #B64A3A
```

Logo language:

- central node = local team
- branching nodes = local staff and proxy people
- repeated smaller branches = self-similar team network
- colored nodes = different execution/ownership modes

## Layout

### Desktop

Recommended layout:

```text
left nav
top node/connection status
center work list
right detail drawer or context panel
```

Desktop should prioritize scanning multiple work states.

### Mobile

Recommended layout:

```text
top node status
single-column cards
bottom tabs
detail sheets
```

Mobile should prioritize:

- accept/reject inbound missions
- view waiting remote work
- submit short deliverables
- check connection health

## Copy Rules

Use product language:

- staff
- local team
- proxy person
- mission
- assignment
- deliverable
- connection
- handoff

Avoid exposing implementation language:

- raw agent graph
- session
- job id as primary label
- callback endpoint
- transport unless in settings

## Accessibility And Clarity

Required:

- badges must include text, not color only
- buttons need explicit labels
- failures must be visible in list and detail views
- long task titles must wrap
- mobile cards must remain readable with one hand

## UI Invariants

1. The user can tell local AI work from proxy human work at a glance.
2. Inbound missions are never hidden inside chat.
3. Returned deliverables are durable artifacts.
4. Connection health is visible before and after dispatch.
5. The UI shows a local team, not a deep agent hierarchy.
6. Every important workflow has an owner action and a system state.

