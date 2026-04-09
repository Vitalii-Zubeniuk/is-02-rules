---
description: "Review a change against Excalidraw package boundaries and dependency direction"
argument-hint: "Describe the planned or implemented change"
---

# Architecture Check Command
Validate this change against project architecture rules:

{{args}}

Requirements:
1. Determine whether logic belongs in `packages/*` or `excalidraw-app/*`.
2. Verify dependency direction is preserved:
   `common -> math -> element -> excalidraw -> excalidraw-app`.
3. Detect deep/internal cross-package imports and suggest public entry-point alternatives.
4. If behavior affects both app and embed consumers, recommend moving implementation to `packages/excalidraw` or lower package.
5. Output:
   - `Verdict`: pass/fail
   - `Violations`: bullet list with file paths
   - `Fix plan`: concrete edits
   - `Docs impact`: whether `dev-docs/` should be updated
