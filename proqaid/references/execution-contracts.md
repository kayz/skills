# PROQAID External Execution Contracts

Read this reference only when an iteration needs host/privileged preparation,
CI/CD, a test platform, a remote environment, or another external executor.

## Human Operator Preparation Packet

Consolidate all requests into one entry-time packet:

| Field | Required content |
|---|---|
| Purpose | decision or exit evidence enabled |
| Timing | blocks development, a wave, or exit |
| Target | OS, architecture, version, account, service, or permission |
| Action | exact operation or copyable command when appropriate |
| Safety | privilege, secret, cost, destructive, and rollback notes |
| Success | deterministic readiness condition |
| Evidence | minimum output, identifier, version, or artifact to return |
| Alternative | CI/CD, IT, remote executor, or approved defer |

The user may perform the packet or delegate it. Do not ask the user to expose
secret values in chat; request only confirmation, identifiers, or safe evidence.

## Environment Contract

Delivery defines requirements, not manual host configuration:

- OS/architecture and tool/runtime versions;
- locks, image digests, licenses, resource and security limits;
- network, account, secret-injection, migration, rollback, and cleanup rules;
- canonical build/start/health/acceptance commands;
- expected outputs and artifact retention; and
- the final preflight and exit checks.

Project-owned CI, container, and validation configuration may be implemented in
the repository. Host installation, long-lived credentials, organization systems,
and privileged configuration go to the Human Operator or external platform.

## Test Execution Contract

Quality defines:

- behavior and acceptance IDs;
- inputs, fixtures, environment, and failure injection;
- required tool capabilities, isolation, concurrency, and timeout;
- exact entry command or machine-readable job;
- pass/fail rules; and
- evidence and retention requirements.

Workers run fast module TDD. Existing CI/CD or test platforms run deterministic
full, real-environment, browser, multi-platform, migration, scan, and
reproducibility jobs. Quality judges whether the result proves the business
contract; it does not reproduce the tool manually.

## Evidence Summary

Every external result must return:

```text
candidate commit/tag
environment identity and versions
tool and command/job identity
test/check inventory and counts
start/end time and exit code
artifact, image, report, and log references with digests where applicable
cleanup result
verdict and failed acceptance IDs
```

Keep raw logs and artifacts in the execution system. Load only the summary into
normal agent context; retrieve exact failure slices when diagnosis is required.

## Autonomous Continuation

After preparation:

1. Run preflight.
2. If it passes, continue without further confirmation.
3. Trigger deterministic jobs asynchronously and release agent slots.
4. Consume the evidence summary when the job completes.
5. Diagnose failures with targeted artifacts; retry safe transient failures.
6. Pause only for new authority, unsafe/destructive action, material scope or
   environment change, no safe tool alternative, or human risk acceptance.

If preflight or final evidence is missing, keep the applicable gate pending.
Never downgrade the target environment or relabel approximate evidence as final.
