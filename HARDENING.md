<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v3.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step 'Upload artifact' references `actions/upload-artifact@v4`, which uses a mutable version tag (`v4`) instead of a pinned 40-character commit SHA. This means the action could be silently updated (or compromised) without the consuming workflow noticing, enabling a supply-chain attack.

Locations:

- `action.yml:72`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced `actions/upload-artifact@v4` (mutable tag) with `actions/upload-artifact@ea165f8d65b6e75b540449e92b4886f43607fa02 # v4` (pinned full commit SHA) in hardened/action/action.yml at line 72.

