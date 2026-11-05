<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v4.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 35eb538..1fda49d 100644
--- a/action.yml
+++ b/action.yml
@@ -21,6 +21,23 @@ outputs:
 runs:
   using: composite
   steps:
     - name: Archive artifact
       shell: sh
       if: runner.os == 'Linux'
@@ -37,7 +54,6 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     # Switch to gtar (GNU tar instead of bsdtar which is the default in the MacOS runners so we can use --hard-dereference)
     - name: Archive artifact
       shell: sh
@@ -55,7 +71,6 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     # Massage the paths for Windows only
     - name: Archive artifact
       shell: bash
@@ -74,10 +89,9 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     - name: Upload artifact
       id: upload-artifact
       uses: actions/upload-artifact@ea165f8d65b6e75b540449e92b4886f43607fa02 # v4.6.2
       with:
         name: ${{ inputs.name }}
         path: ${{ runner.temp }}/artifact.tar

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** prior-fix

**Notes:**

I made no edits because action.yml already has the previous harden's security changes. inputs.path reaches the shell through an env var (INPUT_PATH) and is quoted, so there is no script injection. actions/upload-artifact is pinned to ea165f8d65b6e75b540449e92b4886f43607fa02, which I checked against the v4.6.2 tag. The only visible differences in the prior patch are removed blank lines, which change nothing. The patch's header says it added 17 lines, but their content is missing from the patch, so they could not be re-applied.

