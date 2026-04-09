---
description: "Scaffold a new React component following Excalidraw project conventions"
argument-hint: "Component name and target package/folder (e.g. MyButton packages/excalidraw/components)"
---

Create a new React component following project conventions:

{{args}}

Steps:
1. Parse component name (PascalCase) and target folder from the argument.
2. Determine correct location:
   - Shared/editor UI → `packages/excalidraw/components/`
   - App-only UI → `excalidraw-app/components/`
3. Create `<ComponentName>.tsx` with:
   - Functional component using hooks
   - Explicit typed props interface (`<ComponentName>Props`)
   - No class components
   - No default mutable state unless required
4. Create `<ComponentName>.module.scss` in the same folder for styles (empty placeholder if no styles yet).
5. Export the component from the nearest `index.ts` or note that the consumer must import directly.
6. Add an error boundary wrapper suggestion if the component renders external/async content.
7. Output:
   - Created files with full paths
   - Minimal boilerplate content for each file
   - Import snippet the consumer should use
