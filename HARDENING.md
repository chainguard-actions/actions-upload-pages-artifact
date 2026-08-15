<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v4.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or version strings instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved.

- draft-release.yml: `actions/checkout@v4` (tag)
- publish-immutable-actions.yml: `actions/checkout@v4` (tag), `actions/publish-immutable-action@0.0.3` (version tag)
- release.yml: `actions/publish-action@v0.3.0` (version tag)
- test-hosted-runners.yml: `actions/checkout@v4` (tag), `actions/download-artifact@v4` (tag)

Locations:

- `.github/workflows/draft-release.yml:11`
- `.github/workflows/publish-immutable-actions.yml:14`
- `.github/workflows/publish-immutable-actions.yml:18`
- `.github/workflows/release.yml:22`
- `.github/workflows/test-hosted-runners.yml:21`
- `.github/workflows/test-hosted-runners.yml:31`

### missing-permissions (severity: medium)

draft-release.yml has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. This means the workflow runs with the default (broad) repository permissions.

Locations:

- `.github/workflows/draft-release.yml:1`

### missing-permissions (severity: medium)

test-hosted-runners.yml has no top-level `permissions:` key and its only job (`test`) also has no job-level `permissions:` key. This means the workflow runs with the default (broad) repository permissions.

Locations:

- `.github/workflows/test-hosted-runners.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references by resolving them to full commit SHAs: actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262 (used in draft-release.yml, publish-immutable-actions.yml, test-hosted-runners.yml), actions/publish-immutable-action@0.0.3 → 4b1aa5c1cde5fedc80d52746c9546cb5560e5f53 (publish-immutable-actions.yml), actions/publish-action@v0.3.0 → f784495ce78a41bac4ed7e34a73f0034015764bb (release.yml), actions/download-artifact@v4 → d3f86a106a0bac45b974a628896c90dbdf5c8093 (test-hosted-runners.yml). Added top-level `permissions: contents: read` and job-level permissions to draft-release.yml (with pull-requests: write for release-drafter) and test-hosted-runners.yml (contents: read only). The publish-immutable-actions.yml already had job-level permissions. The release.yml already had a top-level permissions block.

