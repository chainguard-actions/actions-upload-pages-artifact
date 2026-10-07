<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v1.0.4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses `actions/upload-artifact@main`, which is pinned to a mutable branch reference (`main`) rather than an immutable 40-character commit SHA. This means the action could be silently updated (or compromised) without any change to this file, creating a supply-chain risk. It should be pinned to a full SHA, e.g. `actions/upload-artifact@<40-char-sha> # vX.Y.Z`.

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced `actions/upload-artifact@main` (mutable branch reference) with `actions/upload-artifact@cf430e030ddbb5b0abf93d22962f4752f3646cd9 # main` (immutable full commit SHA) in hardened/action/action.yml at line 57. The SHA was resolved via lookup_action_sha for the `main` branch ref.

