# PROQAID Operating Levels

Read this reference after recording the human-selected level. Use Lite only
when explicitly requested; otherwise use Full. Both levels apply the Iteration
Size Gate, Tool-First execution, one Human Operator preparation packet,
candidate-bound evidence, and independent Review.

## Full

Full means all seven authority areas are covered, not that all seven standing
agents are invoked.

- Orchestrator and Review are always independent.
- Invoke Quality for production behavior or acceptance changes.
- Invoke Product for user-visible scope, concepts, or claims.
- Invoke Architecture for public/cross-module domain, data, dependency,
  persistence, or contract changes.
- Invoke Interface for UI, API, CLI, accessibility, or interaction changes.
- Invoke Delivery for build, migration, deployment, CI/CD, runtime, security,
  observability, rollback, or release changes.
- Record an uninvoked role as `not affected`; do not create role memory solely
  to record inactivity.
- Use 2-4 temporary implementation workers when scopes are independent.

Full specialists own their affected decisions. Orchestrator owns shared writes,
scope, routing, integration, Human Operator handoff, and cleanup. Review audits
the impact matrix, Design Freeze, risk waves, evidence, and exit.

## Lite

Lite reduces agent separation, not acceptance integrity.

- Orchestrator and Review remain independent.
- Quality remains independent when production behavior changes and proves the
  integrated business loop.
- Orchestrator covers concise Product, Architecture, and Interface decisions by
  default; invoke a specialist only for a material decision that should not be
  combined.
- Invoke Delivery independently for material deployment, migration, runtime,
  rollback, security, or release work.
- Use one worker by default and at most two with disjoint ownership.
- If Lite becomes insufficient, report the concrete blocker and ask the human;
  never upgrade automatically.

Lite uses the same Human Operator protocol: affected roles provide requirements,
Orchestrator sends one batched preparation packet at entry, the human or another
external operator prepares privileged/host resources, deterministic preflight
verifies readiness, and execution then continues autonomously. Missing final
environment evidence blocks exit unless the human accepts a deviation.

## Role Impact Matrix

Use one compact table in the iteration brief:

| Authority | affected / covered / not affected | owner | required decision/evidence |
|---|---|---|---|
| Product | | | |
| Architecture | | | |
| Interface | | | |
| Quality | | | |
| Delivery | | | |
| Review | affected | Review | admission/exit audit |

Do not create empty role directories, inboxes, outboxes, or context documents.
An invoked role writes only an authority-bearing decision or evidence index with
a named downstream reader.

## Memory

Create only what the current iteration needs:

```text
README.md
iteration-N-checklist.md
.proqaid/
  orchestrator/
    current-iteration.md
    decisions.md       # only unresolved or durable decisions
  <invoked-role>/      # only authority/evidence a later reader needs
docs/<affected-role>/  # current human-facing output only
```

Tool-specific hard constraints remain in their native files when required.
Inbox/outbox files are optional transport and should normally be removed after
their decision is consumed. Raw CI/test/log artifacts remain in the external
execution system; PROQAID stores a compact candidate-bound index.

## Flow Differences

| Concern | Full | Lite |
|---|---|---|
| Specialist decisions | invoke each affected authority | Orchestrator covers P/A/I unless material |
| Quality | independent when behavior/acceptance changes | independent when behavior changes |
| Review | independent, risk-scaled | independent, concise |
| Workers | 2-4 | 1, at most 2 |
| Human preparation | one batched packet + preflight | same |
| Deterministic execution | existing CI/CD/tools first | same |
| Exit evidence | complete applicable gate | complete applicable gate |
