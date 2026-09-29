<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v2.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references `actions/upload-artifact@v3`, which uses a mutable version tag (`@v3`) instead of a pinned 40-character commit SHA. This means the action could be silently updated or replaced by a supply-chain attacker without any change to this repository. It should be pinned to a full SHA, e.g. `actions/upload-artifact@<40-char-sha> # v3`.

Locations:

- `action.yml:63`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/upload-artifact@v3` to full commit SHA `ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5` in hardened/action/action.yml line 63. The mutable tag is preserved as a comment for readability.

