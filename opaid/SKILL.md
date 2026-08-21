---
name: opaid
description: Use for bounded local software implementation that must end with an exact self-tested workspace candidate and concise Human handoff. Work directly by default; use sibling Workers only when at least two independently testable tracks have disjoint write ownership. Excludes remote CI/CD, release tags, deployment, and target-environment operations.
---

# OPAID

## Outcome

Deliver the requested local code change on the real workspace, verify the exact
final candidate with applicable local tests, and give the Human a concise review
surface. Optimize for the lowest total coordination cost, not maximum Agent use.

Use the Root directly by default. Treat the orchestration tool as the operational
source of truth; do not create registries, mailboxes, status ledgers, governance
documents, or extra briefs. Maintain one repository brief only when the user or
repository requires it.

## Boundary

Run from task framing through locally self-tested code and handoff. Do not enter
remote CI/CD, release/version operations, deployment, SIT, UAT, production, or
target-environment administration. If separately requested, finish the local
candidate first and use the applicable release workflow as a separate activity.

Do not add default review, documentation, role, or ceremony phases. Infer the
outcome and acceptance from the request and repository context. Ask only when a
missing semantic or authority decision would materially change the result.

## Execution Mode

Work directly when there is one implementation track, write scopes overlap,
semantics are unsettled, or dispatch and integration are unlikely to save work.

Use Workers only when at least two ready tracks are independently useful, have
frozen inputs, disjoint write ownership, deterministic completion checks, and an
expected saving greater than their coordination cost. Each Worker must produce
code, a bounded diagnosis, or executable test evidence; do not create generic
role, documentation, or review Agents.

When Workers are justified, read
[references/parallel-workers.md](references/parallel-workers.md). Otherwise, do
not load that reference.

## Verification and Recovery

- Before editing, identify the intended outcome and applicable local tests. Record
  non-goals, exact base state, and frozen public contracts only when they constrain
  the change or parallel ownership.
- During implementation, run the narrow affected tests when useful. Each modifying
  Worker runs its targeted check. After the final edit or integration, the Root
  inspects workspace status and diff, then runs the required final local test set
  once against the exact candidate.
- Treat command output and exit status as evidence; prose is not a substitute.
  Report only material test counts, blockers, and risks rather than raw logs.
- Preserve compatible passing work. Repair bounded mechanical failures in place
  and rerun affected tests. Do not restart unrelated work, overwrite user changes,
  weaken tests or acceptance, or manufacture a pass.

## Exit

Finish only when acceptance is met, the final candidate has been inspected, all
required local tests pass, and no unresolved in-scope error remains. If a required
test cannot pass, report the blocking evidence and do not label the work complete.

Return the changed files or concise change summary, actual final test commands and
results, and residual risks. The handoff does not authorize remote CI/CD, release,
or deployment work.
