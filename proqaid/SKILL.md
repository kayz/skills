---
name: proqaid
description: Use when starting, rescuing, or running a software project iteration with standing agents, worker agents, governance documents, acceptance checklists, stale documentation, agent-boundary conflicts, test-stub pollution, hardcoded placeholders, or business-closure verification concerns.
---

# PROQAID

## Overview

PROQAID governs software delivery with user-selected Lite and Full operating
levels. Full uses seven standing roles; Lite invokes a reduced agent set. Both
levels freeze module and contract boundaries before implementation, preserve
only memory with a named downstream reader, and let only
Orchestrator-dispatched workers change production code.

## Operating Level Gate

The human owns the operating-level decision. PROQAID must not infer it from
iteration count, code size, risk, rollback difficulty, project state, or any
other signal.

- If the human explicitly specifies `Lite`, use Lite.
- Otherwise use Full.
- Record the human's selection in
  `.proqaid/orchestrator/current-iteration.md` before dispatching any role or
  worker.
- Never upgrade or downgrade automatically. If the selected level becomes
  insufficient, report the concrete blocker and ask the human whether to
  change levels. Keep the current level until the human answers.

After selecting the level, read
`references/operating-levels.md` for its role model, memory layout, and flow.

## Exclusive Governance Layer

PROQAID is the only planning, orchestration, agent-dispatch, review, and iteration-memory system while it governs an iteration.

- Do not activate Superpowers or another governance workflow for the same iteration, including parallel planning, executing-plan, subagent-development, branch-finishing, or code-review workflows that create a second task graph, agent hierarchy, review cadence, or authority.
- Do not create or update `.superpowers/` artifacts. Treat inherited files as read-only historical evidence; merge any needed fact into a PROQAID authority document, then archive or remove them at the exit gate.
- TDD, worktree isolation, systematic debugging, and verification before completion remain allowed inside PROQAID-assigned worker and gate ownership, but must not create a second plan, status system, dispatcher, or Review owner.
- If another governance workflow owns an active iteration, switch only after explicit human selection and a safe handoff; never run both during transition.

## Role Set

PROQAID means Product, Review, Orchestrator, Quality, Architecture, Interface,
Delivery. The acronym names the system, not the execution order.

| Document Prefix | Role | Owns | Must Not Own |
|---|---|---|---|
| `o` | Orchestrator | Iteration flow, checklist routing, worker dispatch, worktrees, integration, cleanup | Unreviewed unilateral decisions |
| `p` | Product | Users, goals, business concepts, README/user manual, product scope | Internal architecture |
| `r` | Review | Process audit, document hygiene, consistency, evidence checks, corrective findings | Product direction or code fixes |
| `q` | Quality | Acceptance checklist, business-closure tests, test cases, test report | Implementation details |
| `a` | Architecture | Module boundaries, domain model, data flow, API contracts, dependency direction | Interface polish |
| `i` | Interface | UI/API/CLI experience, user flows, interaction states, accessibility | Backend internals |
| `d` | Delivery | Build, run, deploy, configuration, migrations, observability, release risk | Feature scope |

Standing roles may edit only documentation artifacts in their own ownership
area, including their `.proqaid/<role>/` memory and `docs/<role>/` outputs.
They do not edit production code. Production code changes happen through
temporary worker agents dispatched by Orchestrator, preferably in isolated
worktrees.

Standing roles own the authority for their content area, but Orchestrator owns
shared-file writes. When a standing role needs `README.md`, tool constraint
files such as `.codex/AGENTS.md` or `.claude/CLAUDE.md`, the current checklist,
or another role's documentation outputs changed, it proposes the exact change
in `outbox.md`; Orchestrator serially merges approved changes.

## Project Memory Layout

For Full, use this minimum structure unless the project already has an
equivalent convention:

```text
README.md
iteration-N-checklist.md
.codex/
  AGENTS.md
.claude/
  CLAUDE.md
.proqaid/
  orchestrator/
  product/
  review/
  quality/
  architecture/
  interface/
  delivery/
src/
docs/
  product/
    scope.md
  architecture/
    data-dictionary.md
  interface/
    ui-reference.md
    assets/
      example.png
  quality/
    evidence.md
    evidence/
      example.json
  delivery/
    release-notes.md
  review/
    audit-summary.md
hidden/
result/
```

Lite uses the reduced layout in `references/operating-levels.md`.

Keep the project root small. `README.md` and the current
`iteration-N-checklist.md` are the default human entry documents. Tool-facing
constraint files live in the default tool directories, such as
`.codex/AGENTS.md` and `.claude/CLAUDE.md`.

Agent memory belongs under `.proqaid/`. Orchestrator coordination records live
in `.proqaid/orchestrator/`; this is the seventh PROQAID role, not one of the
six fixed production/review agent work directories. Production/review agent
work directories use natural role names without initials or `-role` suffixes:
`product`, `architecture`, `interface`, `quality`, `delivery`, and `review`.
Each `.proqaid/<role>/context.md` must state which current `docs/<role>/`
artifacts the role produces, what each artifact is for, and the allowed file
types or subdirectories for that role.

Human-facing documents are unique latest versions:

- `README.md`: current product, users, content, and user-facing acceptance manual.
- `.codex/AGENTS.md` and `.claude/CLAUDE.md`: equivalent hard constraints for
  the relevant tools.
- `iteration-N-checklist.md`: the only entry and exit contract for the current iteration.
- `docs/<role>/`: role-owned current documentation output directories. Do not
  use `README.md` inside these directories as the default artifact name; the
  role's `.proqaid/<role>/context.md` defines the active artifacts.
- `src/`: source code.
- `hidden/`: sensitive or non-exposed material.
- `result/`: latest compiled, packaged, or release result.

`hidden/`, `result/`, and `src/` are conventional separation points for reduced
project roots: private materials, latest outputs, and source code respectively.

Agent-facing documents must include a validity line such as
`Valid: iteration-3 only` or `Valid: long-term until superseded`.

## Entry Gate

When PROQAID starts, Orchestrator must first run project confirmation with the
human before calling other roles:

- project name and purpose
- target users and current product state
- overall project goal and production target from `README.md`, the current
  checklist, or the human
- planned iteration sequence or current iteration count
- new project, existing governed project, or existing unmanaged project
- current iteration number or need for `iteration-0 baseline recovery`
- this iteration's objective and how it advances the overall goal
- hard constraints that belong in tool constraint files such as
  `.codex/AGENTS.md` and `.claude/CLAUDE.md`
- expected deliverable for this session
- human-selected operating level (`Lite` when explicitly requested; otherwise
  Full)
- whether standing roles should initialize documents now

After confirmation, Orchestrator records the answers and operating level in
`.proqaid/orchestrator/current-iteration.md`. It then initializes only the
roles required by that level.

Orchestrator must reason from the whole project to the current iteration:
overall goal -> iteration sequence -> current iteration objective -> current
checklist. Do not let a single user request or worker task replace the project
goal. If the overall goal, iteration count, or current objective is unclear,
ask the human before dispatching standing roles or workers.

Before feature work, Orchestrator must classify the project:

| Condition | Required Action |
|---|---|
| Empty/new project | Create the PROQAID memory layout and draft initial human documents. |
| Existing project with complete current governance docs | Start from the current checklist. |
| Existing project without reliable governance docs | Run `iteration-0 baseline recovery`; do not add features yet. |

`iteration-0 baseline recovery` produces no business feature. It verifies the
current code reality, separates facts from assumptions, builds or repairs the
human document set, initializes standing-agent contexts, isolates stale docs,
and creates the next real iteration checklist.

## Lite Iteration Flow

Follow `references/operating-levels.md`. Keep Quality and Review independent
and invoke only the Lite agent set. Do not change levels without explicit human
direction.

## Full Iteration Flow

1. Orchestrator confirms the project and iteration with the human.
2. Orchestrator reads `README.md`, the current checklist, tool constraint files
   such as `.codex/AGENTS.md` and `.claude/CLAUDE.md`, and
   `.proqaid/orchestrator/*`.
3. Orchestrator maps the overall production goal to the iteration sequence and
   writes or updates this iteration's objective before decomposing tasks.
4. Orchestrator confirms tool constraint files have equivalent constraints when
   more than one tool is in use.
5. Orchestrator dispatches Product, Architecture, Interface, Quality, and
   Delivery concurrently where their write scopes are disjoint. Each role
   produces only decisions or artifacts named in a downstream consumer's
   brief.
6. Architecture and Interface freeze module ownership, dependency direction,
   public contracts, errors, and deferred behavior. Quality freezes the test
   ownership matrix and cross-module acceptance. Product and Delivery freeze
   affected scope and runtime constraints.
7. Review performs one Design Freeze audit over the authoritative artifacts.
   Resolve blocking and important findings before production workers start.
8. Orchestrator dispatches implementation waves with disjoint module ownership
   and isolated worktrees. Keep ready independent worker slots occupied; do not
   parallelize a dependency or shared write merely to fill capacity.
9. Workers run module-local TDD and regression. Review runs risk-focused fresh
   checks at a wave boundary; Quality runs cross-module business acceptance
   once per integration wave.
10. Orchestrator serially integrates approved wave commits and records only
    decisions, unresolved findings, evidence references, and cleanup needed by
    a later reader.
11. Final Quality, Delivery, and Review run the iteration exit gates. Resolve
    blocking and important findings or move them to a human-approved checklist
    item.
12. Orchestrator exits only after the exit gate passes.

The Orchestrator's implementation-phase job is dispatch, dependency routing,
integration, and exception handling. It must not duplicate worker code work or
create status artifacts solely to demonstrate activity.

## Standing Role Concurrency

Orchestrator should use available concurrency for ready independent roles and
workers after giving each a disjoint write scope. Parallel agents may read
shared documents, but write only inside their assigned directory or worktree.

Conflict prevention rules:

- Orchestrator creates all role inboxes from the same project snapshot.
- Orchestrator does not overwrite a role inbox while that role is running.
- A role brief may be an inbox file or the dispatch message itself. Persist it
  only when another agent, the human, or the exit audit will read it.
- Keep one authoritative current output per decision area. Do not maintain both
  latest and round-stamped copies unless the older round is evidence for an
  unresolved finding or accepted deviation.
- Standing roles never edit shared human-facing documents directly.
- If two role outboxes propose conflicting changes, Orchestrator records the
  conflict in `deviations.md` and routes the decision to the current checklist
  when human judgment is required.
- Do not run two instances of the same standing role concurrently for the same
  iteration unless their write files are explicitly different and named in the
  dispatch.

## Milestone Communication

Do not create or maintain an agent status board by default. Report concise
milestones to the human: design frozen, worker blocked, valid RED, candidate
ready, review verdict, wave integrated, and iteration accepted.

Persist a state transition only when a downstream reader needs it to make a
decision. Put task ownership and dependencies in the implementation plan;
blockers and exceptions in decisions/deviations; test evidence in the worker or
Quality report. Do not duplicate elapsed time, file counts, line counts, or
agent state across governance files.

## Communication Protocol

Standing roles do not message each other directly. All cross-role communication
goes through Orchestrator.

When a persisted inbox is required, keep it to:

```markdown
# Inbox: <role> for iteration-N

## Current Checklist Items
- ...

## Relevant Changes or Questions
- ...

## Required Output
- Update context/review/outbox only.
- Do not edit production code.
```

When a persisted outbox is required, keep it to:

```markdown
# Outbox: <role> for iteration-N

## Scope Checked
- [ ] This iteration affects my role.
- [ ] This iteration does not affect my role.

## Findings
- [blocking] ...
- [important] ...
- [note] ...

## Required Orchestrator Actions
- [ ] Add to checklist: ...
- [ ] Dispatch worker: ...
- [ ] Ask human: ...
- [ ] Update human-facing docs: ...

## Documents Updated
- ...

## Validity
Valid: iteration-N only | long-term until superseded
```

## Worker Rules

Workers are temporary implementation agents. Lite dispatches one worker by
default and at most two with independent write scopes. Full may dispatch 2-4
temporary workers. They may edit production code only inside the ownership and
worktree assigned by Orchestrator.

Before dispatch, freeze a module ownership and test ownership matrix. It names
the public boundary, upstream dependencies, shared-file owner, module-local
tests, and the wave-level consumers that invalidate the module's prior green.

After design freeze, Orchestrator should keep independent implementation slots
occupied. Sequence only dependencies, shared-file writes, and findings that
change another worker's contract.

Worker prompts must name:

- task and checklist item
- write ownership
- files or modules in scope
- files or modules out of scope
- required tests, including business-closure evidence when applicable
- report path
- cleanup expectations

Workers must not leave test-only production methods, fake implementations,
hardcoded demo data, TODO placeholders, throwaway scripts, or unused mocks in
production code. If a temporary artifact is unavoidable during work, it must be
removed or explicitly routed to the checklist before exit.

Within PROQAID-owned worker dispatch, apply TDD, worktree isolation, systematic
debugging, and verification before completion as techniques. Do not activate a
second planning, subagent-development, or review workflow around the worker.

## Test Cadence and Ownership

Assign each test to exactly one primary gate:

| Gate | Owner | Required evidence |
|---|---|---|
| Module TDD | Worker | One valid target RED, same-command GREEN, module regression |
| Risk review | Review | Fresh tests for changed contracts, invariants, security, persistence, money, migration, or replay risk |
| Wave acceptance | Quality | Cross-module business path once after the wave is integrated |
| Runtime acceptance | Delivery | Environment/deployment smoke and stage-specific container checks |
| Iteration exit | Quality (business) + Delivery (runtime) + Review (audit) | One full-suite evidence set, real business closure, final runtime/container acceptance |

Do not rerun an unchanged full suite in Worker, Delivery, Quality, and Review
merely to reproduce the same green result. Verify the evidence-to-commit binding
and rerun only the owning gate or a risk-triggered subset. Invalidate prior
evidence when a public contract, dependency, fixture, migration, toolchain,
runtime image, or test itself changes.

For hybrid environments, run high-frequency language and business tests in the
fixed development environment. Run container-specific checks once at the
applicable stage or exit gate unless container configuration changed.

## Exit Gate

An iteration cannot be called complete until Orchestrator has evidence for each
item:

- Current checklist items are implemented, rejected, or explicitly deferred by
  the human.
- Product confirms `README.md` reflects current user-visible behavior, or in
  Lite the Orchestrator provides Product evidence and Review audits it.
- Architecture confirms changed contracts and boundaries are documented, or in
  Lite the Orchestrator provides Architecture evidence and Review audits it.
- Interface confirms changed UI/API/CLI flows are documented, or in Lite the
  Orchestrator provides Interface evidence and Review audits it.
- Quality confirms tests prove the business loop, not just unit behavior.
- Delivery confirms build/run/deploy implications when independently invoked;
  otherwise Lite records Delivery as not affected and Review verifies that
  classification.
- Review returns `pass` or `pass-with-accepted-findings`.
- Tool constraint files such as `.codex/AGENTS.md` and `.claude/CLAUDE.md`
  remain synchronized when both exist.
- Expired agent documents and temporary worker artifacts are cleaned or archived.
- No test stubs, hardcoded placeholders, or one-off test code remain in
  production code.

## Review Audit

Review is the internal auditor. It reports findings; it does not fix them
directly. Perform one design-freeze Review before implementation, risk-triggered
reviews for high-risk task candidates, one implementation Review per integrated
wave, and a final exit audit. Do not review every micro-task by default.

Review reads the authoritative design freeze, candidate diff, concise worker
evidence, Quality/Delivery evidence, unresolved decisions, and cleanup relevant
to its verdict. It does not read every intermediate role file or repeated copy.
Scale review effort by semantic risk and changed behavior, not document count.

Review findings must use this shape:

```markdown
# Process Audit Report: iteration-N

## Verdict
pass | pass-with-findings | blocked

## Findings
### Blocking
- Problem:
- Evidence:
- Impact:
- Owner:
- Recommended correction:

### Important
- ...

### Minor
- ...

## Required Orchestrator Actions
- [ ] Send to <role>: ...
- [ ] Dispatch worker: ...
- [ ] Ask human: ...
- [ ] Clean artifact: ...

## Evidence Checked
- ...
```

Blocking and important findings cannot be ignored. Orchestrator either routes
them for correction or records a human-approved deviation in the current
checklist.

## Document Hygiene

Default to fewer documents. Before creating a governance document, name its
downstream reader and the decision or action it enables. If no downstream
reader exists, do not create it. A document may exist only when it is a
human-facing latest document, an authority-bearing current artifact, evidence
for an unresolved/accepted finding, or an explicitly archived historical
record.

Docs are role-owned current documentation output directories, not fixed agent
memory. Agent memory belongs under `.proqaid/`; each role's memory must explain
which `docs/<role>/` outputs it produces and why. Docs artifacts may be
Markdown, JSON, CSV, images, PDFs, or other valid current artifacts when the
owner and purpose are clear. Sensitive or non-exposed materials still belong
under `hidden/`; latest compiled or release outputs belong under `result/`;
source code belongs under `src/`.

Do not default to one role file per role such as `docs/p-product.md`, and do
not use `docs/<role>/README.md` as the default artifact. Use named artifacts
and subdirectories that describe the product, such as
`docs/interface/ui-reference.md`, `docs/architecture/data-dictionary.md`,
`docs/quality/evidence.md`, `docs/quality/evidence/*.json`, or
`docs/interface/assets/*.png`.

At iteration end:

- merge durable facts into the owning current document
- mark role documents with updated validity
- delete or archive stale drafts, worker scratch files, abandoned reports, and
  obsolete subagent folders
- preserve only evidence needed to understand the current system or accepted
  deviations

If a document is not current, owned, valid, or archived, treat it as pollution.

## Common Mistakes

| Mistake | Correction |
|---|---|
| Entering Lite without an explicit human instruction | Use Full; PROQAID never selects Lite itself. |
| Automatically changing Lite or Full after discovering new facts | Report the blocker and ask the human; keep the selected level until answered. |
| Starting feature work in a half-finished project with no reliable docs | Run iteration-0 baseline recovery first. |
| Letting standing roles edit code | Route code work through Orchestrator-dispatched workers. |
| Keeping every generated report | Merge durable facts, then clean or archive the source artifact. |
| Maintaining status files with no decision consumer | Report milestones in conversation; persist only plan, blocker, evidence, or decision data a later reader needs. |
| Re-running the same full suite in every role | Assign a primary test gate; rerun risk subsets and one wave/final full gate. |
| Starting workers before boundaries are frozen | Freeze Architecture, Interface, errors, dependencies, deferred behavior, and test ownership; then run design Review. |
| Treating green unit tests as acceptance | Require Quality evidence for the user/API/interface business loop. |
| Letting roles resolve conflicts privately | Send conflicts to Orchestrator deviations and checklist decisions. |
| Letting Orchestrator self-certify completion | Require Review audit before exit. |
