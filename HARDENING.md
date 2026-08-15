<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 10 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Three `run:` blocks in action.yml directly interpolate `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` inside shell command strings. The expression is expanded by the YAML template engine before the shell executes the command, allowing an attacker-controlled input value to inject shell metacharacters or arguments. All three Archive artifact steps (Linux, macOS, Windows) are affected.

Locations:

- `action.yml:30`
- `action.yml:44`
- `action.yml:57`

### unpinned-uses (severity: high)

Unpinned `uses:` references found in draft-release.yml: `actions/checkout@v4` (mutable tag, not a 40-char commit SHA). These references can silently change if the tag is moved, enabling supply-chain attacks.

Locations:

- `.github/workflows/draft-release.yml:11`

### unpinned-uses (severity: high)

Unpinned `uses:` references found in publish-immutable-actions.yml: `actions/checkout@v4` (mutable tag) and `actions/publish-immutable-action@0.0.3` (mutable version tag). Neither is pinned to a full 40-character commit SHA.

Locations:

- `.github/workflows/publish-immutable-actions.yml:16`
- `.github/workflows/publish-immutable-actions.yml:19`

### unpinned-uses (severity: high)

Unpinned `uses:` reference found in release.yml: `actions/publish-action@v0.3.0` (mutable version tag, not a 40-char commit SHA).

Locations:

- `.github/workflows/release.yml:22`

### unpinned-uses (severity: high)

Unpinned `uses:` references found in test-hosted-runners.yml: `actions/checkout@v4` (appears twice, mutable tag) and `actions/download-artifact@v4` (appears twice, mutable tag). None are pinned to a full 40-character commit SHA.

Locations:

- `.github/workflows/test-hosted-runners.yml:22`
- `.github/workflows/test-hosted-runners.yml:31`
- `.github/workflows/test-hosted-runners.yml:57`
- `.github/workflows/test-hosted-runners.yml:66`

### missing-permissions (severity: medium)

draft-release.yml has no top-level `permissions:` key and the `draft-release` job also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository default (often `write-all`), granting broader access than necessary.

Locations:

- `.github/workflows/draft-release.yml:1`

### missing-permissions (severity: medium)

test-hosted-runners.yml has no top-level `permissions:` key and neither the `test` job nor the `test-include-hidden` job has a job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository default, granting broader access than necessary.

Locations:

- `.github/workflows/test-hosted-runners.yml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}" appears directly in run: block of step "Archive artifact"; move to env: map

Locations:

- `action.yml:39`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}" appears directly in run: block of step "Archive artifact"; move to env: map

Locations:

- `action.yml:57`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}" appears directly in run: block of step "Archive artifact"; move to env: map

Locations:

- `action.yml:75`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all findings:
1. action.yml: Moved `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` out of all three run: blocks into env: blocks as EXCLUDE_HIDDEN. Used `${EXCLUDE_HIDDEN:+"$EXCLUDE_HIDDEN"}` in shell to safely pass the optional flag without injecting an empty argument.
2. draft-release.yml: Pinned actions/checkout@v4 to SHA 34e114876b0b11c390a56381ad16ebd13914f8d5. Added top-level permissions (contents: read) and job-level permissions (contents: read, pull-requests: write for release-drafter).
3. publish-immutable-actions.yml: Pinned actions/checkout@v4 to SHA 34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/publish-immutable-action@0.0.3 to SHA 4b1aa5c1cde5fedc80d52746c9546cb5560e5f53.
4. release.yml: Pinned actions/publish-action@v0.3.0 to SHA f784495ce78a41bac4ed7e34a73f0034015764bb.
5. test-hosted-runners.yml: Pinned all four unpinned uses (2x actions/checkout@v4, 2x actions/download-artifact@v4) to their full SHAs. Added top-level permissions (contents: read) and job-level permissions (contents: read) for both test jobs.

