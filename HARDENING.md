<!-- markdownlint-disable -->

# Hardening Report: actions--upload-pages-artifact/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--upload-pages-artifact/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

A composite step pins its action to a version tag instead of a full 40-character commit SHA: `uses: actions/upload-artifact@v4`. A tag can be moved to point at different code.

Locations:

- `action.yml:77`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index a19976e..1e7b3ba 100644
--- a/action.yml
+++ b/action.yml
@@ -21,6 +21,23 @@ outputs:
 runs:
   using: composite
   steps:
     - name: Archive artifact
       shell: sh
       if: runner.os == 'Linux'
@@ -36,7 +53,6 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     # Switch to gtar (GNU tar instead of bsdtar which is the default in the MacOS runners so we can use --hard-dereference)
     - name: Archive artifact
       shell: sh
@@ -53,7 +69,6 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     # Massage the paths for Windows only
     - name: Archive artifact
       shell: bash
@@ -71,10 +86,9 @@ runs:
         echo ::endgroup::
       env:
         INPUT_PATH: ${{ inputs.path }}
-
     - name: Upload artifact
       id: upload-artifact
       uses: actions/upload-artifact@v4
       with:
         name: ${{ inputs.name }}
         path: ${{ runner.temp }}/artifact.tar

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, prior-fix

**Notes:**

Pinned actions/upload-artifact@v4 to ea165f8d65b6e75b540449e92b4886f43607fa02 (# v4). The earlier patch's diff listed added lines without showing what they were. Its only visible changes were removed blank lines and the same upload-artifact reference. In the current action.yml, inputs.path already reaches each archive step through env as INPUT_PATH and is used quoted. Nothing else needed changing.

