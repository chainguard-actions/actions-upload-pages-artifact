<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v5.0.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) is violated: a GitHub Actions expression is placed directly inside run blocks. Each of the three 'Archive artifact' steps (Linux at line 39, macOS at line 57, Windows at line 75) contains this line: `${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} \`. The expression can only produce one of two fixed strings, so the attacker cannot choose what text is inserted. Even so, it is filled in by template substitution before the shell parses the script, which the check forbids. Fix: compute the flag in the shell from an env var, e.g. `INCLUDE_HIDDEN: ${{ inputs.include-hidden-files }}` and then `if [ "$INCLUDE_HIDDEN" != "true" ]; then set -- --exclude='.[^/]*'; fi` (or the equivalent).

Locations:

- `action.yml:39`
- `action.yml:57`
- `action.yml:75`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 82713b4..2a44101 100644
--- a/action.yml
+++ b/action.yml
@@ -25,63 +25,95 @@ outputs:
 runs:
   using: composite
   steps:
     - name: Archive artifact
       shell: sh
       if: runner.os == 'Linux'
+      env:
+        INPUT_PATH: ${{ inputs.path }}
+        INCLUDE_HIDDEN_FILES: ${{ inputs.include-hidden-files }}
       run: |
         echo ::group::Archive artifact
+        if [ "$INCLUDE_HIDDEN_FILES" = "true" ]; then
+          EXCLUDE_HIDDEN=""
+        else
+          EXCLUDE_HIDDEN="--exclude=.[^/]*"
+        fi
         tar \
           --dereference --hard-dereference \
           --directory "$INPUT_PATH" \
           -cvf "$RUNNER_TEMP/artifact.tar" \
           --exclude=.git \
           --exclude=.github \
-          ${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} \
+          ${EXCLUDE_HIDDEN} \
           .
         echo ::endgroup::
-      env:
-        INPUT_PATH: ${{ inputs.path }}
-
     # Switch to gtar (GNU tar instead of bsdtar which is the default in the MacOS runners so we can use --hard-dereference)
     - name: Archive artifact
       shell: sh
       if: runner.os == 'macOS'
+      env:
+        INPUT_PATH: ${{ inputs.path }}
+        INCLUDE_HIDDEN_FILES: ${{ inputs.include-hidden-files }}
       run: |
         echo ::group::Archive artifact
+        if [ "$INCLUDE_HIDDEN_FILES" = "true" ]; then
+          EXCLUDE_HIDDEN=""
+        else
+          EXCLUDE_HIDDEN="--exclude=.[^/]*"
+        fi
         gtar \
           --dereference --hard-dereference \
           --directory "$INPUT_PATH" \
           -cvf "$RUNNER_TEMP/artifact.tar" \
           --exclude=.git \
           --exclude=.github \
-          ${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} \
+          ${EXCLUDE_HIDDEN} \
           .
         echo ::endgroup::
-      env:
-        INPUT_PATH: ${{ inputs.path }}
-
     # Massage the paths for Windows only
     - name: Archive artifact
       shell: bash
       if: runner.os == 'Windows'
+      env:
+        INPUT_PATH: ${{ inputs.path }}
+        INCLUDE_HIDDEN_FILES: ${{ inputs.include-hidden-files }}
       run: |
         echo ::group::Archive artifact
+        if [ "$INCLUDE_HIDDEN_FILES" = "true" ]; then
+          EXCLUDE_HIDDEN=""
+        else
+          EXCLUDE_HIDDEN="--exclude=.[^/]*"
+        fi
         tar \
           --dereference --hard-dereference \
           --directory "$INPUT_PATH" \
           -cvf "$RUNNER_TEMP\artifact.tar" \
           --exclude=.git \
           --exclude=.github \
-          ${{ inputs.include-hidden-files != 'true' && '--exclude=.[^/]*' || '' }} \
+          ${EXCLUDE_HIDDEN} \
           --force-local \
           "."
         echo ::endgroup::
-      env:
-        INPUT_PATH: ${{ inputs.path }}
-
     - name: Upload artifact
       id: upload-artifact
       uses: actions/upload-artifact@bbbca2ddaa5d8feaa63e36b76fdaad77386f024f # v7.0.0
       with:
         name: ${{ inputs.name }}
         path: ${{ runner.temp }}/artifact.tar

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, prior-fix

**Notes:**

In all three 'Archive artifact' steps (Linux, macOS, Windows), replaced the inline ${{ inputs.include-hidden-files ... }} expression with INCLUDE_HIDDEN_FILES env var; the shell builds the optional --exclude='.[^/]*' flag via POSIX 'set --' and passes "$@" to tar/gtar, so an empty value adds no argument and the pattern is not glob-expanded. Behavior matches upstream.

