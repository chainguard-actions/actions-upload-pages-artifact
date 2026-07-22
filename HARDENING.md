<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags instead of full 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the tag is moved. Failing references:
- action.yml: `actions/upload-artifact@v4`
- .github/workflows/draft-release.yml: `actions/checkout@v4`, `release-drafter/release-drafter@v5`
- .github/workflows/release.yml: `actions/publish-action@v0.3.0`
- .github/workflows/test-hosted-runners.yml: `actions/checkout@v4`, `actions/download-artifact@v4`

Locations:

- `action.yml:47`
- `.github/workflows/draft-release.yml:10`
- `.github/workflows/draft-release.yml:11`
- `.github/workflows/release.yml:20`
- `.github/workflows/test-hosted-runners.yml:22`
- `.github/workflows/test-hosted-runners.yml:32`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions. Each file should declare minimal required permissions explicitly.
- .github/workflows/draft-release.yml: no permissions declared at top-level or job level
- .github/workflows/test-hosted-runners.yml: no permissions declared at top-level or job level

Locations:

- `.github/workflows/draft-release.yml:1`
- `.github/workflows/test-hosted-runners.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 6 unpinned action references to full 40-character commit SHAs (with tag comments for readability): actions/upload-artifact@v4, actions/checkout@v4, release-drafter/release-drafter@v5, actions/publish-action@v0.3.0, and actions/download-artifact@v4. Added top-level permissions blocks to draft-release.yml (contents: write, pull-requests: write for release-drafter) and test-hosted-runners.yml (contents: read for minimal access). release.yml already had a permissions block and only needed the unpinned action fixed.

