<!-- markdownlint-disable -->

# Hardening Report: mdecoleman--pr-branch-name/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mdecoleman--pr-branch-name/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if a tag is moved or a dependency is compromised.

Failing references:
- check-dist.yml: actions/checkout@v4, actions/setup-node@v4, actions/upload-artifact@v4
- ci.yml: actions/checkout@v4, actions/setup-node@v4
- codeql-analysis.yml: actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/autobuild@v3, github/codeql-action/analyze@v3
- linter.yml: actions/checkout@v4, actions/setup-node@v4, super-linter/super-linter/slim@v6

All should be pinned to their full SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:32`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/ci.yml:22`
- `.github/workflows/ci.yml:28`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:44`
- `.github/workflows/linter.yml:24`
- `.github/workflows/linter.yml:30`
- `.github/workflows/linter.yml:41`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a `run:` shell command string. The offending line is:

  run: echo "${{ steps.test-action.outputs.time }}"

The value of `steps.test-action.outputs.time` is substituted into the shell command before the shell parses it, allowing an attacker who can influence that output value to inject arbitrary shell commands. The value should be passed via an `env:` variable and then referenced as a quoted shell variable, e.g.:

  env:
    ACTION_TIME: ${{ steps.test-action.outputs.time }}
  run: echo "$ACTION_TIME"

Locations:

- `.github/workflows/ci.yml:55`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 12 unpinned action references across 4 workflow files by pinning to full SHA digests: actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4 → ea165f8d65b6e75b540449e92b4886f43607fa02, github/codeql-action/{init,autobuild,analyze}@v3 → 4187e74d05793876e9989daffde9c3e66b4acd07, super-linter/super-linter/slim@v6 → 1fa6ba58a88783e9714725cf89ac26d53e80c148. Fixed script injection in ci.yml by moving ${{ steps.test-action.outputs.time }} into an env: block as ACTION_TIME and referencing it as $ACTION_TIME in the shell command.

