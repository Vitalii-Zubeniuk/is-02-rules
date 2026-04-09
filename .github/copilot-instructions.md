# Project coding standards

## Generic Communication Guidelines

- Be succint and be aware that expansive generative AI answers are costly and slow
- Avoid providing explanations, trying to teach unless asked for, your chat partner is an expert
- Stop apologising if corrected, just provide the correct information or code
- Prefer code unless asked for explanation
- Stop summarizing what you've changed after modifications unless asked for

## TypeScript Guidelines

- Use TypeScript for all new code
- Where possible, prefer implementations without allocation
- When there is an option, opt for more performant solutions and trade RAM usage for less CPU cycles
- Prefer immutable data (const, readonly)
- Use optional chaining (?.) and nullish coalescing (??) operators

## React Guidelines

- Use functional components with hooks
- Follow the React hooks rules (no conditional hooks)
- Keep components small and focused
- Use CSS modules for component styling

## Project Architecture Rules

- Keep reusable/editor domain logic inside `packages/*`; keep app wiring/integration logic inside `excalidraw-app/*`
- Prefer changes in `packages/excalidraw` (or lower-level packages) when behavior should apply to both the app and embedded package consumers
- Respect dependency direction: `common` -> `math` -> `element` -> `excalidraw` -> `excalidraw-app`; do not introduce reverse dependencies
- Avoid cross-package deep imports from internal files; prefer public exports/index entry points
- If architecture is affected, update the relevant docs in `dev-docs/` and mention the package boundary in code comments/PR notes

## Naming Conventions

- Use PascalCase for component names, interfaces, and type aliases
- Use camelCase for variables, functions, and methods
- Use ALL_CAPS for constants

## Error Handling

- Use try/catch blocks for async operations
- Implement proper error boundaries in React components
- Always log errors with contextual information

## Testing

- Always attempt to fix #problems
- Always offer to run `yarn test:app` in the project root after modifications are complete and attempt fixing the issues reported

## Types

- Always include `packages/math/src/types.ts` in the context when your write math related code and always use the Point type instead of { x, y}
