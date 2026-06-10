<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: Three `run:` blocks in action.yml directly interpolate `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` into the shell command string. This is a `${{ inputs.* }}` expression embedded directly in a `run:` block, which flows through YAML template substitution before the shell processes it. An attacker controlling the `include-hidden-files` input could inject shell metacharacters. The fix is to move the conditional logic into the shell script itself using the env-var pattern with proper quoting, rather than interpolating the expression directly into the command.

Locations:

- `action.yml:36`
- `action.yml:51`
- `action.yml:66`

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

Fixed all three script injection findings in action.yml. In each of the Linux (sh), macOS (sh), and Windows (bash) 'Archive artifact' steps, removed the direct ${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} expression from the run: block. Replaced it with: (1) an INCLUDE_HIDDEN_FILES env var set to ${{ inputs.include-hidden-files }}, and (2) shell-level if/else logic that sets a local EXCLUDE_HIDDEN variable, then uses ${EXCLUDE_HIDDEN:+"$EXCLUDE_HIDDEN"} to safely pass the optional tar flag only when non-empty.

