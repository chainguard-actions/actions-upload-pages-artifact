# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In all three 'Archive artifact' run: steps (Linux, macOS, Windows), the attacker-controlled input `inputs.include-hidden-files` is interpolated directly into the shell command string via `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}`. This is not routed through an `env:` variable first, meaning a malicious caller can supply a crafted value for `include-hidden-files` that breaks out of the expression context and injects arbitrary shell commands. The safe pattern would be to assign the input to an env var and evaluate it in shell logic instead.

Locations:

- `action.yml:37`
- `action.yml:50`
- `action.yml:65`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed script injection in all three 'Archive artifact' steps (Linux, macOS, Windows) in action.yml. The attacker-controlled `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` expression was removed from all three run: blocks. Instead, each step now maps `inputs.include-hidden-files` to an `INCLUDE_HIDDEN_FILES` env variable, and uses shell if/else logic to set a local `EXCLUDE_HIDDEN` variable that is safely passed to the tar command. This eliminates the shell injection vector in all three locations.

