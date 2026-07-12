---
name: proqaid
description: Use when starting, rescuing, or running a software project iteration with standing roles, worker agents, governance documents, acceptance checklists, stale documentation, agent-boundary conflicts, oversized scope, repeated reviews, environment setup delays, test-stub pollution, hardcoded placeholders, or business-closure concerns.
---

# PROQAID

## Core Principle

PROQAID governs project delivery while spending agent reasoning only where
judgment is required. Use existing deterministic tools for mechanical work,
activate only affected roles, bound iteration size before dispatch, and verify
real business closure with evidence bound to the candidate.

## Operating Level Gate

The human selects the operating level. Use Lite only when explicitly requested;
otherwise use Full. Never change levels automatically. Record the selection in
the current iteration brief, then read `references/operating-levels.md`.

Full covers all seven authority areas but does not mean invoking all seven
standing agents. Lite combines more specialist authority under Orchestrator.
Both keep Review independent and use the Human Operator and Tool-First rules.

## Exclusive Governance Layer

PROQAID is the only planning, orchestration, dispatch, review, and iteration
memory system while it governs an iteration.

- Do not activate Superpowers or another governance workflow for the same iteration.
- Do not create or update `.superpowers/`; inherited files are read-only evidence to archive or remove at exit.
- TDD, worktree isolation, systematic debugging, and verification are worker techniques, not a second plan, dispatcher, status system, or Review owner.
- Switch governance systems only after explicit human selection and a safe handoff.

## Authority Areas

| Role | Owns | Activation trigger |
|---|---|---|
| Orchestrator | scope, size gate, routing, integration, cleanup | always |
| Product | users, value, concepts, README claims | user-visible scope or behavior changes |
| Architecture | modules, domain, data, dependency and contract boundaries | a public or cross-module boundary changes |
| Interface | UI, API, CLI flows and accessibility | an interaction surface changes |
| Quality | acceptance, business closure and evidence judgment | production behavior or acceptance changes |
| Delivery | build, migration, runtime, CI/CD, observability and release risk | an operational contract changes |
| Review | independent audit and findings | always; depth follows risk |

An uninvoked role is coverage, not an agent. Orchestrator records `not affected`
or the covering authority in the iteration brief; do not create an inbox,
outbox, context file, or agent merely to say that nothing changed. Review audits
the classification. Invoke an affected specialist once by default and reactivate
it only when its frozen boundary is invalidated.

Standing roles decide and document inside their authority; they do not edit
production code. Orchestrator-dispatched temporary workers own code changes in
isolated scopes.

## Iteration Size Gate

Before detailed planning, role dispatch, or environment work, Orchestrator must
write a short admission decision: one outcome, one business acceptance sentence,
affected authorities, worker ownership zones, high-risk boundaries, dependency
chain, external prerequisites, and exit evidence.

Propose a split before dispatch when any condition holds:

- the iteration contains more than one independently valuable or verifiable outcome;
- it combines a new foundation/environment baseline with its first full business feature;
- it needs more than three independent write-ownership zones;
- it changes three or more high-risk boundary classes such as public contracts, migration, persistence, authorization, money, replay, or security;
- its critical path needs more than three serial role/worker handoffs;
- its exit condition cannot be stated as one business sentence; or
- completion requires unrelated stateful environments or separate full evidence sets.

Return the recommended sub-iterations, dependency order, parallel opportunities,
and final integration relationship. Wait for human confirmation; do not silently
continue with an oversized iteration.

## Entry and Impact Gate

1. Confirm project purpose, production goal, current state, iteration objective,
   non-goals, operating level, deliverable, and hard constraints.
2. Classify the project as new, governed, or unmanaged. An unmanaged project
   requires iteration-0 baseline recovery before feature work.
3. Run the Iteration Size Gate.
4. Build the role impact matrix and invoke only affected roles for the selected level.
5. Have Architecture, Interface, Quality, and Delivery define only affected
   boundaries, acceptance, and runtime requirements.
6. Consolidate all privileged or external prerequisites into one Human Operator
   preparation packet while affected roles finish design in parallel.
7. Review the authoritative Design Freeze once. Start workers only after
   blocking/important findings are resolved and required preflight passes.

## Human Operator

Treat the human as an available external executor for host installation,
privileged commands, accounts, secrets, approvals, organization systems, and
environment preparation that agents should not perform. The human may delegate
the packet to IT, CI/CD, or another operator.

Send one batched preparation packet per iteration, not piecemeal requests. It
contains purpose, timing, target/version, exact action or copyable command,
security notes, success criteria, minimum returned evidence, alternative, and
whether the item blocks development or only exit. For the packet and evidence
shape, read `references/execution-contracts.md` only when external preparation
or deterministic execution is affected.

After the human reports readiness, run a deterministic preflight and continue
autonomously. Default human interaction is one scope/preparation batch at entry
and one acceptance/release decision at exit. Pause again only for:

- new authority, secret, account, or privileged/destructive action;
- genuine product/scope ambiguity;
- a material environment-contract change;
- repeated tool failure with no safe alternative; or
- human acceptance of a deviation, delay, or release risk.

Code defects, dependency conflicts, ordinary test failures, retryable network
errors, and internal scheduling problems are not reasons to interrupt the human.

## Tool-First Execution

Deterministic checks must use existing tools instead of consuming context to
simulate them. Prefer, in order: project tooling, existing CI/CD or test
platforms, project-owned scripts/configuration, then an external executor.

Agents define requirements, trigger available tools, and judge evidence. They
do not default to installing host software, creating WSL distributions,
configuring long-lived secrets, clicking through environment setup, manually
imitating a linter/test/scanner, or babysitting logs. Repository-owned CI files,
Dockerfiles, Compose, test configuration, and verification scripts may be
implemented by workers when they are in iteration scope.

Use local workspaces for fast module TDD and diagnosis. Offload full regression,
real environments, browser matrices, multi-platform builds, migrations,
container acceptance, SBOM, license, vulnerability, secret, and reproducibility
checks to existing CI/CD or another deterministic executor whenever available.
Run external work asynchronously and release agent slots while it runs.

External evidence has two layers:

1. a compact summary with exact commit, environment/tool versions, command,
   test inventory/count, exit code, artifact/image digest, cleanup, and verdict;
2. raw logs, reports, screenshots, traces, and scan artifacts stored outside
   normal agent context.

Read the summary first. Load only relevant raw failure slices when diagnosis or
evidence sufficiency requires them. A green dashboard without candidate-bound
evidence is not acceptance.

Environment requirements are frozen early but may be prepared later. Missing
final environment evidence leaves the state `implementation complete; environment
acceptance pending` and blocks exit unless the human explicitly accepts a deviation.

## Design Freeze

Freeze only affected boundaries in one authoritative brief. Include module
ownership, public contracts, errors, identity/version, time/effective rules,
idempotency/concurrency, transaction and persistence proof, published-content
read behavior, migration/legacy-data policy, security, observability, deferred
behavior, test ownership, and runtime requirements when relevant.

If one late semantic defect appears, route a targeted correction. A second in
the same boundary stops micro-fixes and triggers one focused re-freeze. A third
cross-module semantic defect triggers a size-gate reassessment and a split or
human-approved defer; do not grow an endless correction chain.

## Implementation Waves

- Full may dispatch 2-4 workers; Lite uses one by default and at most two.
- Give every worker a checklist item, write scope, excluded scope, frozen public
  boundary, required RED/GREEN/regression evidence, handoff, and cleanup rule.
- Keep independent ready slots occupied. Serialize shared writes and dependency chains.
- Workers use module-local TDD and must not leave fake behavior, hardcoded demo
  success, test-only production methods, TODO placeholders, or throwaway scripts.
- Orchestrator dispatches, routes dependencies, integrates, and handles exceptions;
  it does not duplicate worker implementation or testing.

## Test and Review Ownership

| Gate | Primary owner | Evidence |
|---|---|---|
| Module TDD | Worker + project tool | valid RED, same-command GREEN, module regression |
| Risk review | Review | candidate diff and fresh changed-risk subset |
| Wave acceptance | Quality + external tool | one integrated cross-module business path |
| Runtime acceptance | Delivery + external tool/operator | target environment and operational contract |
| Exit | Quality, Delivery, Review | one candidate-bound full evidence set and audit |

Do not rerun an unchanged full suite across roles. Invalidate evidence only when
its public contract, dependency, fixture, migration, toolchain, runtime image,
or test changes. Review once at Design Freeze, at a changed high-risk wave, and
at exit; do not review every micro-task.

## Context and Document Budget

- Keep one current iteration brief/checklist and one authoritative decision per area.
- Give roles only relevant README sections, the current brief, affected contracts,
  unresolved decisions, and candidate evidence; do not default-read all `.proqaid/`.
- Role output is a decision delta for a named downstream reader, not repeated background.
- Do not maintain an agent status board or duplicate latest/stamped transport files.
- Keep raw deterministic output as external artifacts and human/agent docs as concise indexes.
- Archive or remove stale drafts, worker scratch, obsolete role memory, and inherited governance artifacts at exit.

## Exit Gate

Exit only when:

- every checklist item is implemented, rejected, or explicitly deferred by the human;
- current user-visible claims and affected boundaries match verified behavior;
- Quality proves the business loop, not only unit success;
- Delivery verifies applicable environment, build, migration, runtime, rollback,
  observability, and release requirements;
- Review returns `pass` or `pass-with-accepted-findings`;
- tool constraints remain synchronized where multiple tools are used;
- temporary workers, environments, data, and stale documents are cleaned; and
- production code contains no test stubs, fake success, hardcoded placeholders,
  or one-off test paths.

Blocking and important findings must be corrected or explicitly accepted by the
human. Orchestrator cannot self-certify completion.

## Common Mistakes

| Mistake | Correction |
|---|---|
| Full invokes every role automatically | Full covers every authority; invoke only affected roles. |
| Planning an epic as one iteration | Apply the size gate before role or worker dispatch. |
| An agent configures the host to prove requirements | Send one Human Operator packet; verify with preflight. |
| Agents simulate lint/test/scan work in prose | Use the existing deterministic tool and inspect evidence. |
| Loading complete logs and governance history | Read compact indexes first and load only relevant slices. |
| Repeating full tests in Worker, Quality, Delivery, and Review | Give each check one primary gate. |
| Reopening roles after every small fix | Reinvoke only when their frozen boundary changed. |
| Green CI is treated as business acceptance | Require exact candidate, environment, command, inventory, and artifact binding. |
| Environment is unavailable at exit | Remain pending or obtain explicit human deviation; do not claim completion. |
