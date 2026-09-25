<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string in three steps. The expression `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }}` is substituted into the shell command before the shell processes it. Although the expression evaluates to one of two literal strings, the `inputs.include-hidden-files` value is caller-controlled and the template substitution happens at the YAML level before the shell ever quotes or validates it. This pattern should be replaced by moving the conditional logic into the shell script itself using the env-var approach (e.g., set `INCLUDE_HIDDEN: ${{ inputs.include-hidden-files }}` in `env:` and then use `if`/`case` inside the `run:` block).

Locations:

- `action.yml:35`
- `action.yml:49`
- `action.yml:63`

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

Fixed all three 'Archive artifact' steps (Linux/sh, macOS/sh, Windows/bash) by: (1) removing the inline ${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} expression from each run: block, (2) adding INCLUDE_HIDDEN_FILES: ${{ inputs.include-hidden-files }} to each step's env: block, and (3) replacing the inline expression with a POSIX-compatible if/else block that sets EXCLUDE_HIDDEN to either '--exclude=.[^/]*' or '' and uses ${EXCLUDE_HIDDEN} (unquoted for word-splitting) in the tar command.

