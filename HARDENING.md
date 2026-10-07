<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v1.0.4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step 'Upload artifact' uses `actions/upload-artifact@main`, which references the mutable `main` branch instead of a pinned 40-character SHA commit hash. If the upstream repository is compromised or the branch is force-pushed, this action could execute arbitrary malicious code in all workflows that use this action.

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced `actions/upload-artifact@main` (mutable branch reference) with `actions/upload-artifact@cf430e030ddbb5b0abf93d22962f4752f3646cd9 # main` (pinned full commit SHA) in hardened/action/action.yml at line 57.

