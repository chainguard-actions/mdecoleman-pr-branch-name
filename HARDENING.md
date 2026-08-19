<!-- markdownlint-disable -->

# Hardening Report: mdecoleman--pr-branch-name/1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mdecoleman--pr-branch-name/1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references GitHub Actions by mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved or the upstream action is compromised. Failing references: `actions/checkout@v2` and `actions/setup-node@v2`. Both should be pinned to their full SHA digests (e.g. `actions/checkout@<40-char-sha> # v2`).

Locations:

- `.github/workflows/push-to-master.yaml:11`
- `.github/workflows/push-to-master.yaml:12`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/push-to-master.yaml` has no top-level `permissions:` key and the only job (`build-and-commit`) also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (often `write-all`), granting unnecessarily broad access. A minimal permissions block (e.g. `contents: write` for the git push step) should be added.

Locations:

- `.github/workflows/push-to-master.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned actions/checkout@v2 to SHA 0717577d45739eb3c851188b29f50ed6c0b2194e and actions/setup-node@v2 to SHA 7c12f8017d5436eb855f1ed4399f037a36fbd9e8 (both with # v2 comments for readability). Added a top-level `permissions: contents: write` block — the minimum required for the git push step in the build-and-commit job.

