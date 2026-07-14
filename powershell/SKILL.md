---
name: powershell
description: Use when composing, reviewing, or debugging Windows PowerShell commands or .ps1 scripts where paths, quoting, environment variables, native exit codes, or Bash/Linux shell habits could cause failures.
---

# PowerShell

## Purpose

Produce native, directly executable PowerShell. Use PowerShell 7 by default;
target Windows PowerShell 5.1 only when the task explicitly requires it. Separate
cross-platform implementations instead of mixing Bash, WSL, or macOS syntax into
a `.ps1` file.

## Native Syntax

- Use `$env:NAME` for environment variables.
- Use quoted Windows paths or `Join-Path`:

```powershell
$project = "C:\Users\User\My Project"
$child = Join-Path $project "src"
```

- Use `Get-ChildItem`, `Copy-Item`, `Move-Item`, `Remove-Item`, and `New-Item`
  instead of Unix aliases or commands in scripts.
- Use `-LiteralPath` for filesystem mutations. Before a recursive delete or move,
  resolve and verify that the target remains inside the intended root.
- Do not emit `export`, `VAR=value command`, `source`, `chmod`, `rm -rf`, Bash
  heredocs, `/tmp`, `/home`, or WSL path assumptions.
- Do not use Bash command substitution. PowerShell subexpressions use `$()` but
  have PowerShell semantics.

## Sequencing and Errors

Do not treat `;` as a success-conditional replacement for Bash `&&`; it always
continues. Prefer explicit control flow.

For cmdlets, make terminating behavior explicit:

```powershell
$ErrorActionPreference = 'Stop'
try {
    Copy-Item -LiteralPath $source -Destination $destination
} catch {
    Write-Error $_
    exit 1
}
```

For native programs, inspect `$LASTEXITCODE`:

```powershell
& git status --short
if ($LASTEXITCODE -ne 0) {
    exit $LASTEXITCODE
}
```

Avoid long inline command chains. Put repeated or multi-step automation in a
reviewable `.ps1` entry point. When PowerShell 7 is required, invoke it explicitly:

```powershell
pwsh -NoProfile -NonInteractive -File "C:\path\to\script.ps1"
```

## Runtimes and Activation

Use Windows runtime layouts. For a Python virtual environment:

```powershell
& ".\venv\Scripts\Activate.ps1"
```

Do not translate it to `source venv/bin/activate`. If an action needs administrator
rights or a policy change, state the requirement and safe boundary; do not silently
elevate or weaken execution policy.

## Parse Before Execution

For a generated non-trivial script, parse it before running:

```powershell
$tokens = $null
$errors = $null
[System.Management.Automation.Language.Parser]::ParseFile(
    $scriptPath,
    [ref]$tokens,
    [ref]$errors
) | Out-Null

if ($errors.Count -gt 0) {
    $errors | ForEach-Object { Write-Error $_.Message }
    exit 1
}
```

## Final Check

Confirm that the output uses one PowerShell dialect, quoted Windows-compatible
paths, `$env:` variables, correct cmdlet/native error handling, no Bash/WSL
assumptions, and safe literal filesystem targets. Run the parser and the requested
verification command; return the actual exit code and evidence.
