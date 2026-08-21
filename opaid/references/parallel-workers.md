# OPAID Parallel Workers

Read this reference only after the execution gate in `SKILL.md` justifies Workers.

## Dispatch

- Create only direct sibling Workers; Workers do not create descendants.
- Use the smallest context that makes the contract self-contained:
  `fork_turns="none"` when the contract contains everything needed, otherwise the
  fewest recent turns. Use full history only when the task genuinely depends on it.
- Partition by independent outcomes and disjoint paths. Give a shared file one
  owner at a time. Do not fill or backfill slots merely because they are available.
- Default communication is Worker-to-Root. Send peer messages only for a fact that
  blocks a declared dependency; avoid status chatter.

## Worker Contract

Include only what affects execution:

- bounded outcome and allowed or excluded paths;
- exact base only when stale-state risk exists;
- frozen behavior or interfaces the Worker may not change;
- required targeted check and forbidden changes.

Require a concise return containing status, changed files, commands and exit
status, plus material blockers or residual risk. Do not request copied diffs,
transcripts, plans, or narrative summaries when the workspace and command output
already provide that evidence.

## Integration and Recovery

The Root accepts a result only after inspecting the actual workspace diff and test
evidence. On timeout or bounded failure, interrupt the attempt, preserve compatible
verified work, and reassign only the unfinished remainder. Rerun only affected
checks during repair, followed by the final required test set once on the exact
integrated candidate.
