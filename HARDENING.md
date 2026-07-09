<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--upload-pages-artifact/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references use mutable tags instead of full 40-character SHA commit hashes, making the action vulnerable to supply-chain attacks if the tag is moved. Failing references: action.yml: `actions/upload-artifact@v4`; .github/workflows/draft-release.yml: `actions/checkout@v4`, `release-drafter/release-drafter@v5`; .github/workflows/release.yml: `actions/publish-action@v0.3.0`; .github/workflows/test-hosted-runners.yml: `actions/checkout@v4`, `actions/download-artifact@v4`.

Locations:

- `action.yml:56`
- `.github/workflows/draft-release.yml:11`
- `.github/workflows/draft-release.yml:12`
- `.github/workflows/release.yml:22`
- `.github/workflows/test-hosted-runners.yml:22`
- `.github/workflows/test-hosted-runners.yml:31`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/draft-release.yml:1`
- `.github/workflows/test-hosted-runners.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full SHA hashes: actions/upload-artifact@v4 → ea165f8d65b6e75b540449e92b4886f43607fa02 (action.yml), actions/checkout@v4 → 34e114876b0b11c390a56381ad16ebd13914f8d5 (draft-release.yml and test-hosted-runners.yml), release-drafter/release-drafter@v5 → 09c613e259eb8d4e7c81c2cb00618eb5fc4575a7 (draft-release.yml), actions/publish-action@v0.3.0 → f784495ce78a41bac4ed7e34a73f0034015764bb (release.yml), actions/download-artifact@v4 → d3f86a106a0bac45b974a628896c90dbdf5c8093 (test-hosted-runners.yml). Added minimal permissions blocks: draft-release.yml gets 'contents: write' and 'pull-requests: write' (needed for release-drafter to create/update draft releases and PRs); test-hosted-runners.yml gets 'contents: read' (only needs to read the repo).

