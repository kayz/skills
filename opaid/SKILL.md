---
name: opaid
description: Use for local software development from a frozen task through an exact self-tested candidate and concise Human handoff, especially when multiple independent implementation, diagnosis, or test tracks can use direct sibling subagents or interrupted work must resume from verified state. Excludes remote CI/CD, release tags, deployment, and target-environment operations.
---

# OPAID

## Purpose

Deliver the exact final code candidate with its specified local self-tests passing
and hand it to the Human in a concise review surface.
Maximize useful parallel subagent work while preserving verified progress. Keep one
Root Orchestrator as the authority for scope, scheduling, semantics, integration,
and final verification; use direct sibling Workers for bounded execution.

Treat the orchestration tool as the operational source of truth. Do not create an
Agent registry, mailbox, status ledger, governance checklist, or process document
unless the user explicitly requests that artifact.

When the repository requires one iteration brief, maintain exactly that one Human
document. Do not turn Worker messages, command transcripts, intermediate candidate
notes, or subcycles into additional tracked documents.

## Boundary

Run OPAID from task framing through locally self-tested code and its Human handoff.
Exclude remote CI/CD, release tags, deployment, target-environment administration,
SIT, UAT, and production operations. If the user separately requests one of those
activities, finish OPAID first and enter the repository's release workflow as a
separate task; do not make it an OPAID exit gate.

Do not add a default review, documentation, role, or ceremony phase. Add a bounded
analysis or read-only review task only when it directly resolves a current code or
self-test risk.

## Round Contract

Before dispatch, establish:

- one code outcome, acceptance sentence, non-goals, and exact base state;
- the specified local self-test commands and risk-based regression commands;
- a dependency graph of bounded tasks with allowed and excluded write paths;
- frozen public contracts and the decisions that only the Root may change.

Each Worker contract contains one task, exact base, allowed paths, required result,
self-test commands, forbidden changes, timeout, and evidence fields. Require the
Worker to return changed files, commands actually run, exit codes, test counts when
available, blockers, and residual risk. Worker prose never substitutes for command
evidence.

## Parallel Execution

1. Partition work by independent outcomes and disjoint write ownership, not by
   arbitrary file counts. Keep shared files under one owner at a time.
2. Fill available slots immediately with the highest-value ready Worker tasks.
   Only the Root creates Agents; keep every Worker as its direct child and prohibit
   descendant creation.
3. Keep the Root clean for inspection, decisions, scheduling, integration, and the
   final self-test. Let the Root implement only when no useful Worker task can use
   the available slot or the change cannot be safely separated.
4. Let sibling Workers exchange facts, questions, proposals, and evidence directly.
   A peer message cannot change scope, acceptance, a public contract, write
   ownership, or final candidate identity; return those decisions to the Root.
5. When a slot opens, backfill it with the highest-value ready implementation,
   defect-fix, integration, or test task. Do not invent review work merely to keep a
   slot busy.
6. Let the Root integrate only completed, bounded results. Inspect the actual diff
   and runner evidence before accepting a Worker completion statement.

Use the Root sequentially when dispatch overhead, write overlap, or unresolved
semantics makes parallel execution slower or unsafe. Maximum useful parallelism,
not maximum Agent count, is the objective.

## Forward-Only Recovery

Preserve every verified result that remains compatible with the frozen contracts.
For a bounded mechanical failure, keep the candidate and let the same Worker repair
it within its task. On timeout, interrupt the attempt, inspect its diff and evidence,
then reassign only the unfinished bounded remainder. On dependency or contract
change, invalidate and rerun only affected downstream tasks.

Never restart the whole round because one Worker fails, discard unrelated passing
work, overwrite another Worker's files, weaken an assertion, remove a test, or
change acceptance to manufacture a pass. If a required semantic change exceeds the
round contract, stop that path and ask the user for the missing decision while
continuing independent safe work.

## Integration and Exit

After all modifying Workers finish or are stopped:

1. Establish the exact integrated candidate from the real workspace and inspect its
   status and diff for ownership violations, conflicts, debug residue, or unrelated
   edits.
2. Run every specified self-test against that exact candidate. Worker-branch or
   pre-integration passes are supporting evidence only.
3. Route each failure to one bounded Worker when parallel repair is safe; preserve
   passing work and rerun the affected tests plus required regression commands.
4. Update the single repository brief, when required, with only the objective,
   acceptance, non-goals, public-contract changes, Human decisions, actual final
   test evidence, and residual risks.
5. Finish only when the acceptance sentence is satisfied, every specified self-test
   passes on the final candidate, no unresolved in-scope error remains, and the
   concise Human handoff is ready.

Return the changed code, a concise change summary, and actual test commands and
results through the brief or equivalent review surface. The Human may then require
one full local validation plus manual retest on the same candidate, or accept the
documented evidence and continue to merge or the next iteration. Neither choice
implicitly authorizes remote CI/CD. If a specified self-test cannot pass, report
the blocking evidence and do not label the round complete. Do not create a release
tag, wait for remote CI/CD, or report remote CI/CD status as part of the OPAID round.

## Quick Corrections

| Drift | Correction |
|---|---|
| Root implements while ready Worker work exists | Return the Root to orchestration and fill the Worker slot |
| A Worker creates another Agent | Stop the descendant and flatten ownership under the Root |
| Finished slots become generic reviewers | Backfill ready code, fix, integration, or test work first |
| One failure restarts all tracks | Preserve compatible passing results and rerun the affected subgraph |
| Worker says tests passed without facts | Require actual command, exit-code, and inventory evidence |
| Human must read Agent working notes | Collapse the result into the single brief or concise handoff |
| Local work expands into CI/CD | Exit with the self-tested candidate and invoke the release workflow separately only after Human authorization |
