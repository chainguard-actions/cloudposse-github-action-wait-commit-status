<!-- markdownlint-disable -->

# Hardening Report: cloudposse--github-action-wait-commit-status/v0.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cloudposse--github-action-wait-commit-status/v0.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags or branch names instead of pinned 40-character commit SHAs. This exposes the action to supply-chain attacks where a tag or branch could be silently updated to run malicious code.

Failing references:
- branch.yml: `cloudposse/.github/.github/workflows/shared-github-action.yml@main`
- release.yml: `cloudposse/.github/.github/workflows/shared-release-branches.yml@main`
- test-positive.yml: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`
- test-negative.yml: `actions/checkout@v3`, `nick-fields/assert-action@v1`
- test-increased-wait-time.yml: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`
- test-wrong-status.yml: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`

Locations:

- `.github/workflows/branch.yml:19`
- `.github/workflows/release.yml:10`
- `.github/workflows/test-positive.yml:44`
- `.github/workflows/test-positive.yml:50`
- `.github/workflows/test-positive.yml:62`
- `.github/workflows/test-positive.yml:79`
- `.github/workflows/test-negative.yml:37`
- `.github/workflows/test-negative.yml:49`
- `.github/workflows/test-increased-wait-time.yml:44`
- `.github/workflows/test-increased-wait-time.yml:50`
- `.github/workflows/test-increased-wait-time.yml:63`
- `.github/workflows/test-increased-wait-time.yml:80`
- `.github/workflows/test-wrong-status.yml:44`
- `.github/workflows/test-wrong-status.yml:50`
- `.github/workflows/test-wrong-status.yml:63`
- `.github/workflows/test-wrong-status.yml:80`
- `.github/workflows/test-wrong-status.yml:93`

### missing-permissions (severity: medium)

The workflow file `test-negative.yml` has no top-level `permissions:` key and none of its jobs define a `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access to contents and other scopes), violating the principle of least privilege.

Locations:

- `.github/workflows/test-negative.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 6 workflow files by replacing mutable tags/branches with pinned 40-character commit SHAs (preserving original tags in comments). Added missing top-level permissions block (contents: read, statuses: write) to test-negative.yml. All SHAs were resolved using lookup_action_sha.

