# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Three `run:` steps in action.yml directly interpolate the attacker-controlled expression `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` inside shell commands. An attacker supplying a crafted value for the `include-hidden-files` input could inject arbitrary shell commands. The value must be assigned to an environment variable via `env:` and referenced as `$ENV_VAR` in the shell, rather than being interpolated directly into the `run:` block.

Locations:

- `action.yml:38`
- `action.yml:54`
- `action.yml:70`

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

Fixed all three occurrences of script injection in action.yml (Linux, macOS, and Windows Archive artifact steps). The GitHub Actions expression `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` was removed from all `run:` blocks and moved to an `env:` variable `EXCLUDE_HIDDEN_FILES_FLAG` in each step. In the shell scripts, the variable is referenced as `${EXCLUDE_HIDDEN_FILES_FLAG:+"$EXCLUDE_HIDDEN_FILES_FLAG"}` which safely expands to the value only when non-empty, preventing word-splitting and shell injection attacks.

