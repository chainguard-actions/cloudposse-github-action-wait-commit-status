<!-- markdownlint-disable -->

# Hardening Report: cloudposse--github-action-wait-commit-status/v0.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cloudposse--github-action-wait-commit-status/v0.2.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit.

Failing references:
- `.github/workflows/branch.yml`: `cloudposse/.github/.github/workflows/shared-github-action.yml@main`
- `.github/workflows/release.yml`: `cloudposse/.github/.github/workflows/shared-release-branches.yml@main`
- `.github/workflows/test-positive.yml`: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`
- `.github/workflows/test-negative.yml`: `actions/checkout@v3`, `nick-fields/assert-action@v1`
- `.github/workflows/test-increased-wait-time.yml`: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`
- `.github/workflows/test-wrong-status.yml`: `juliangruber/sleep-action@v2.0.0`, `myrotvorets/set-commit-status-action@master`, `actions/checkout@v3`, `nick-fields/assert-action@v1`, `myrotvorets/set-commit-status-action@master`

Locations:

- `.github/workflows/branch.yml:18`
- `.github/workflows/release.yml:9`
- `.github/workflows/test-positive.yml:30`
- `.github/workflows/test-positive.yml:35`
- `.github/workflows/test-positive.yml:50`
- `.github/workflows/test-positive.yml:65`
- `.github/workflows/test-negative.yml:31`
- `.github/workflows/test-negative.yml:46`
- `.github/workflows/test-increased-wait-time.yml:30`
- `.github/workflows/test-increased-wait-time.yml:35`
- `.github/workflows/test-increased-wait-time.yml:51`
- `.github/workflows/test-increased-wait-time.yml:66`
- `.github/workflows/test-wrong-status.yml:31`
- `.github/workflows/test-wrong-status.yml:36`
- `.github/workflows/test-wrong-status.yml:52`
- `.github/workflows/test-wrong-status.yml:67`
- `.github/workflows/test-wrong-status.yml:79`

### missing-permissions (severity: medium)

Four workflow files have no top-level `permissions:` key and no job-level `permissions:` blocks on any of their jobs. Without explicit permissions, workflows inherit the default repository permissions (which may be broad), violating the principle of least privilege.

Locations:

- `.github/workflows/test-positive.yml:1`
- `.github/workflows/test-negative.yml:1`
- `.github/workflows/test-increased-wait-time.yml:1`
- `.github/workflows/test-wrong-status.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving them to full 40-character commit SHAs:
- juliangruber/sleep-action@v2.0.0 → @0ec9cf4f8d053aae56e4b402438184a31c6e87d3
- myrotvorets/set-commit-status-action@master → @09b487ee4d14cbba9b85e5e41dee083ddfe940b5
- actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26
- nick-fields/assert-action@v1 → @1e012cc9f1bf73ccc96470b56c8887478c647e8a
- cloudposse/.github shared workflows @main → @4e05ff6c113efa9322288cedbc7c8950c22616cc

Added top-level `permissions:` blocks to the 4 test workflow files (test-positive.yml, test-negative.yml, test-increased-wait-time.yml, test-wrong-status.yml) with minimal permissions: `contents: read` and `statuses: read`. The branch.yml and release.yml already had permissions blocks so only the unpinned-uses fix was needed for those files.

