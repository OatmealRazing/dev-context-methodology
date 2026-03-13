# Dev Context Methodology (DCM)

A framework for structuring context between humans and AI assistants across software projects.

---

## 1. Overview

Dev Context Methodology (DCM) is a structured approach to managing the information your AI assistant needs to be useful. Rather than dumping everything into every prompt, DCM organizes context into discrete layers, each loaded at the right time and scoped to the right level. It pairs that layered context model with task contracts, session handoff, a project lifecycle, and a lightweight tracker system. The result is an agent that stays oriented, produces consistent output, and degrades gracefully when information is missing -- without requiring large context windows or repeated re-explanation.

---

## 2. Five-Layer Context Model

DCM organizes context into five layers. The agent reads down the layers and stops as soon as it has what it needs for the current task.

| Layer | Name | Question it answers | Target size |
|-------|------|---------------------|-------------|
| 0 | Identity layer | Who am I working with? | ~500 tokens |
| 1 | Workspace layer | What environment is this? | ~400 tokens |
| 2 | Project layer | What project am I in? | ~300-800 tokens |
| 3 | Session layer | What am I working on right now? | Loaded selectively |
| 4 | Artifact layer | What am I working with? | Loaded selectively |

**Layer 0 -- Identity layer:** Persistent facts about the person or team: preferences, conventions, and anything the agent should always know. Loaded automatically at the start of every session.

**Layer 1 -- Workspace layer:** Orientation for the working environment. Describes how projects are organized, what tools are in use, and the operating rules for the space. Loaded automatically.

**Layer 2 -- Project layer:** Project-specific context: stack, build commands, naming conventions, and current state. Loaded when the agent is scoped to a project.

**Layer 3 -- Session layer:** Active task state: what is being worked on, decisions made so far, and the next step. Lives inside the project. Loaded when resuming work on an in-progress task.

**Layer 4 -- Artifact layer:** Session artifacts -- specs, plans, debug logs, intermediate outputs. Loaded only when explicitly relevant to the current step.

**Token discipline:** Layers 0 through 2 combined should stay under 2,000 tokens. These are targets, not hard limits. The goal is to keep always-loaded context lean so the agent has room to work.

**One-way references:** A higher-numbered layer may reference a lower-numbered one. The reverse is not allowed. Same-layer references are fine.

---

## 3. Task Contracts

A task contract is a lightweight definition of how a task type should be executed. Every contract follows a consistent shape: Inputs, Process, and Outputs.

- **Inputs:** What the agent needs before starting (spec, reproduction steps, scope, etc.)
- **Process:** The steps to complete the task, including a verification step
- **Outputs:** What gets produced (code, changelog, findings doc, etc.)

DCM defines four default contract types:

| Task type | Process steps | Notes |
|-----------|---------------|-------|
| feature | Spec > Implement > Verify | Requires a written spec as input |
| bugfix | Reproduce > Diagnose > Fix > Verify | Requires a confirmed reproduction case |
| refactor | Scope > Implement > Verify | Scope must be explicitly bounded before starting |
| release | Changelog > Version > Build > Publish | Follows the project's release checklist |

Two task types are intentionally contractless:

- **Spike:** Timebox, then research, then produce a findings document. The goal is learning, not shipping.
- **Chore:** A discrete task with no process overhead. Must meet the project's definition of done.

All contracts include a verification step. No contract is complete without it.

---

## 4. Session Handoff

DCM tracks active task state at the project level so work can be paused and resumed cleanly.

- One active task per project at a time
- Active task state is stored in a dedicated session file
- Switching tasks: move the current session file to a paused state
- Resuming a task: restore the paused session file to active
- Completing a task: clear the session file; do not archive it as ongoing state

The session file captures: what is being done, decisions made, blockers, and the immediate next step. It should be written as if handing off to another person who has no context. The goal is zero re-orientation time when resuming.

---

## 5. Project Lifecycle

Every project moves through eight stages. Each stage has gate criteria -- conditions that must be satisfied before the project advances to the next stage. Gate criteria are defined per-project and documented in the tracker.

| Stage | Description |
|-------|-------------|
| Idea | Concept identified, not yet evaluated |
| Planning | Scope, approach, and feasibility being determined |
| Setup | Repository, tooling, and dependencies being established |
| Developing | Active feature development underway |
| Shipping | Feature complete; in testing, review, or release prep |
| Maintaining | Shipped and in production; receiving fixes and improvements |
| Paused | Work stopped intentionally; context preserved |
| Archived | No longer active; retained for reference only |

Stages move forward by default. A project can move to Paused from any active stage. Moving to Archived is a lifecycle event, not a failure. The agent flags when gate criteria appear to be met but does not advance the stage without confirmation.

---

## 6. Tracker System

The tracker is the source of truth for project state. It sits outside any individual project and covers the full workspace.

**Backlogs** hold work items that are not in an active sprint. Each item has a title, size estimate, priority (MoSCoW), and optionally a description and acceptance criteria. Backlog items are reviewed and prioritized during sprint planning.

**Sprints** are fixed-scope work cycles. A sprint has a goal, a list of committed items, and a record of what was completed and what was deferred. Sprints are bounded -- scope does not grow mid-sprint without an explicit scope change.

**Retrospectives** are written after each sprint. They capture what worked, what did not, and any process changes to carry forward. The prevention fields from incident logs feed directly into retrospective process changes.

**Incidents** are self-reported by the agent (see Section 7). They are logged to the project's incident log and referenced in retrospectives.

**Metrics** track velocity, completion rates, and incident frequency over time. Metrics are descriptive, not prescriptive. They inform planning but do not drive it.

**Insights** are observations that emerge from metrics and retrospectives. An insight is a pattern worth acting on, not a one-off.

---

## 7. Incident Self-Reporting

The agent self-reports incidents proactively. An incident is anything that disrupted work or produced incorrect output. Logging is not punitive -- it is the mechanism for process improvement.

Five incident types:

| Type | When to log |
|------|-------------|
| agent-error | The agent took a wrong approach, produced bad output, or had to redo work |
| agent-correction | The user corrected the agent's behavior or approach |
| contract-failure | A task contract's verification step failed |
| stage-block | A stage gate could not be met |
| scope-change | Unplanned work disrupted a committed sprint |

Each incident record includes: type, a description of what happened, and a prevention field. The prevention field is a short statement of what would have avoided the incident. Prevention entries feed directly into retrospective process changes.

The agent logs incidents at the time they occur, not at the end of a sprint. A pattern of the same incident type is a signal to revisit the relevant contract or gate criteria.

---

## 8. Quality Guardrails

DCM enforces a small set of quality rules that apply across all projects.

**Token discipline:**
- Layers 0-2 combined: under 2,000 tokens
- Tracker context files: under 80 lines
- Reference files: under 200 lines

These keep always-loaded context lean and leave room for actual work.

**Naming conventions:** Every project follows a consistent naming convention defined at the workspace level. File names, branch names, and identifiers use the same scheme throughout.

**Single canonical home:** Every piece of information lives in exactly one place. If the same fact appears in two files, one of them is wrong. The agent does not duplicate information across layers.

**Plain English:** Write context files as if handing off to a new team member. No jargon. No acronyms without definition on first use. Short sentences. Active voice.

**No em dashes:** Use a comma, a period, or a semicolon instead. Em dashes create ambiguity in context files and make sentences harder to parse programmatically.

**Verification step:** Every task contract must include a verification step in its Process section. An unverified output is not a complete output.

---

## 9. Estimation Scale

DCM uses a five-point size scale for estimating task effort. Sizes are relative, not absolute. A team should calibrate what each size means for their pace.

| Size | Meaning |
|------|---------|
| XS | Minutes. Trivial change, no decisions required. |
| S | A few minutes to an hour. Straightforward, no unknowns. |
| M | A few hours. Clear approach, some decisions to make. |
| L | A full session or more. Significant scope or unknowns present. |
| XL | Multiple sessions. Should probably be broken down further. |

XL items should be treated as a signal to decompose before committing to a sprint. If an item cannot be broken down, it should be treated as a spike first.

---

## 10. Priority Systems

DCM uses two distinct priority systems: one for ranking projects relative to each other, and one for ranking backlog items within a sprint.

**Project priority** (cross-project ranking):

| Level | Weight | Description |
|-------|--------|-------------|
| high | 300 | Critical path; blocks other work or has a hard deadline |
| medium | 200 | Important; should be progressed regularly |
| low | 100 | Nice to have; work on when capacity allows |

Weights are used by the tracker to sort and surface projects. A high-priority project with recent activity outranks a medium-priority project with stale state.

**Backlog item priority** (MoSCoW, for sprint planning):

| Level | Description |
|-------|-------------|
| Must | Required for the current cycle to succeed |
| Should | Important and expected unless a trade-off is needed |
| Could | Desirable; included if capacity allows |
| Won't | Explicitly excluded this cycle; reasoning documented |

Won't items are not rejected -- they are intentionally deferred. The documented reasoning prevents them from being re-litigated each sprint. At retrospective time, Won't items should be reviewed for promotion to a future cycle.
