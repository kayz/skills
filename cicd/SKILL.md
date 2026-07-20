---
name: cicd
description: Use when a Human has explicitly chosen a version number or requested version delivery, release-candidate CI, immutable image publication, test-environment deployment, rollback, or diagnosis of that version pipeline. Enforces a version-tag gate after OPAID and does not run for ordinary branch pushes, Pull Requests, or main merges.
---

# CICD

## Purpose

Turn one Human-authorized version candidate into reproducible remote evidence and
immutable deliverables. Keep ordinary iteration development in OPAID; CICD starts
only when the Human explicitly chooses the version or asks to operate an existing
version pipeline.

## Entry Contract

Before mutating GitHub or a release system, require all of the following:

- an explicit Human version decision or an explicit request to inspect or retry an
  already existing version;
- the repository's accepted version format;
- an exact candidate commit already on the protected default branch;
- the local OPAID handoff and residual risks needed to interpret remote results;
- the intended environment and the authorized extent of side effects.

Do not infer version authority from an ordinary push, Pull Request, merge, local
test pass, Agent plan, or iteration brief. If the version or authority is absent,
stop before creating, moving, or deleting a tag and ask the Human.

## Version Gate

1. Resolve the current default-branch tip and requested version before creating a
   tag.
2. Create the version tag only when the Human has authorized that exact version.
   Treat creation as authorization for the repository's documented automatic
   version CI, image build, scan, and test-environment delivery.
3. Require the tag to identify the exact default-branch tip. Reject worktree diffs,
   feature branches, arbitrary reachable commits, and moving branch names.
4. Treat every version tag as immutable. Never force-move or reuse a failed tag.
   Repair through a new OPAID iteration and a forward-only version.
5. Manual dispatch or retry must resolve to the same existing version tag and exact
   commit; it must not accept an unversioned commit shortcut.

## Automated Delivery Contract

Once the Human creates the tag, allow the repository's automatic line to perform:

1. full Linux CI and supply-chain checks on the tagged commit;
2. build and push images identified first by exact Commit SHA;
3. scan every required image and collect provenance and SBOM evidence;
4. only after the complete image set passes, promote those same manifests to the
   immutable version tag;
5. deploy the test environment using SHA identities;
6. run health checks, smoke tests, deployment recording, and rollback on failure.

Do not publish mutable `latest` or `test-latest` identities. Do not promote a
partially successful build matrix, and do not describe local OPAID evidence as
remote Linux, registry, supply-chain, or target-environment proof.

Production release remains a separate Human-authorized action even when a version
has passed the test environment. Respect repository-specific Environment approvals,
secrets, operational boundaries, and rollback contracts.

## Result Handoff

Report the version, exact commit, workflow identity, immutable artifacts, environment
result, rollback outcome, blockers, and residual risks. Keep GitHub Actions, the
registry, and target-environment deployment records as operational facts; do not
create a parallel release ledger unless the repository explicitly requires one.

## Quick Corrections

| Drift | Correction |
|---|---|
| PR or main merge starts full CI | Restrict the workflow to the Human version gate |
| Agent invents a version | Stop and obtain the Human version decision |
| Tag points to an older reachable commit | Reject it; require the current protected default-branch tip |
| Failed tag is moved after a fix | Preserve it and create a forward-only version after OPAID |
| Matrix publishes a shared mutable tag incrementally | Publish SHA images first and promote only after all scans pass |
| Test deployment is treated as production approval | Require a separate Human production gate |
