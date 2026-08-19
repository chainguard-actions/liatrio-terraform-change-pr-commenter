<!-- markdownlint-disable -->

# Hardening Report: liatrio--terraform-change-pr-commenter/v1.14.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **liatrio--terraform-change-pr-commenter/v1.14.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

- action-test.yml: `actions/checkout@v4`, `actions/setup-node@v4` (used in 3 jobs)
- codeql-analysis.yml: `actions/checkout@v4`, `github/codeql-action/init@v3`, `github/codeql-action/autobuild@v3`, `github/codeql-action/analyze@v3`
- conventional-commits.yml: `liatrio/github-actions/conventional-pr-title@master` (branch ref — highest risk)
- release.yml: `actions/checkout@v4`, `actions/setup-node@v4`
- reusable-terraform-test.yml: `actions/checkout@v4`, `actions/setup-node@v4`

All should be pinned to a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/action-test.yml:20`
- `.github/workflows/action-test.yml:21`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:42`
- `.github/workflows/codeql-analysis.yml:49`
- `.github/workflows/codeql-analysis.yml:63`
- `.github/workflows/conventional-commits.yml:13`
- `.github/workflows/release.yml:9`
- `.github/workflows/release.yml:10`
- `.github/workflows/reusable-terraform-test.yml:19`
- `.github/workflows/reusable-terraform-test.yml:20`

### missing-permissions (severity: medium)

The workflow file `conventional-commits.yml` has no top-level `permissions:` key and no job-level `permissions:` key on its only job (`validate`). Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write-all` if the repo default is set that way). A minimal permissions block such as `permissions: pull-requests: read` should be added.

Locations:

- `.github/workflows/conventional-commits.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full 40-character commit SHAs across 5 workflow files: action-test.yml (3 jobs), codeql-analysis.yml, conventional-commits.yml, release.yml, and reusable-terraform-test.yml. Added top-level `permissions: pull-requests: read` to conventional-commits.yml. All SHAs were resolved using lookup_action_sha and verified with a final search confirming no mutable tag references remain.

