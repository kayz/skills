# PROQAID Operating Levels

Read this reference after recording the human-selected operating level. Use
Lite only when the human explicitly requests it; otherwise use Full.

## Lite Role Model

- Orchestrator owns confirmation, checklist flow, shared writes, and concise
  Product, Architecture, and Interface decisions unless an area is materially
  affected.
- Quality is always independent and proves the business loop.
- Review is always independent and performs the exit audit.
- Invoke Delivery independently for production deployment, migration, runtime,
  or rollback work.
- Invoke Product, Architecture, or Interface only for a material decision that
  Orchestrator and Review should not combine.
- Use one implementation worker by default; use at most two with independent
  write scopes.

An uninvoked role is coverage, not an agent. Do not create its directory,
inbox, outbox, or review. Record it as `covered by Orchestrator` or `not
affected`. Never merge Quality or Review into Orchestrator.

## Lite Flow

1. Confirm the project and record the human's explicit Lite instruction.
2. Create one checklist covering implementation, acceptance, and deployment or
   delivery.
3. Record Product, Architecture, and Interface decisions in Orchestrator memory;
   invoke a specialist only when materially affected.
4. Freeze affected module/API boundaries and test ownership, then have Review
   audit the design freeze when Architecture or Interface is materially changed.
5. Dispatch one worker by default, or two with disjoint ownership. Use both
   slots when independent work is ready.
6. Have workers run module-local TDD; have Quality verify the integrated
   business loop once, not repeat every worker suite.
7. Have Delivery check real deployment, migration, runtime, and rollback work
   without duplicating unchanged business tests.
8. Update shared human documents from verified facts and have Review audit the
   wave/final candidate, evidence, and cleanup.
9. Resolve findings within Lite. If the selected agent set becomes insufficient,
   report the blocker and ask the human whether to remain Lite or switch Full;
   never switch automatically.
10. Exit only after the applicable gate passes.

## Full Role Model and Flow

Invoke all seven standing roles. Use the Design Freeze, reader-driven memory,
parallel worker waves, test ownership, and wave/final audit flow in `SKILL.md`.

## Memory Layouts

Lite creates only current human documents, Orchestrator, Quality, Review, and
independently invoked specialists:

```text
README.md
iteration-N-checklist.md
.codex/AGENTS.md
.claude/CLAUDE.md
.proqaid/
  orchestrator/
  quality/
  review/
  delivery/       # only when independently invoked
  <specialist>/   # only when independently invoked
src/
docs/
hidden/
result/
```

Full creates the complete role memory:

```text
.proqaid/
  orchestrator/
    current-iteration.md
    implementation-plan.md
    decisions.md
    deviations.md
    cleanup.md
  product/
  architecture/
  interface/
  quality/
  delivery/
  review/
```

Each invoked role keeps only authority-bearing current artifacts and evidence
with a named downstream reader. Inbox/outbox files are optional transport, not
mandatory memory. Current human-facing artifacts remain under `docs/<role>/`;
agent memory remains under `.proqaid/<role>/`.
