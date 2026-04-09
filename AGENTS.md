# AGENTS.md — Excalidraw Monorepo

AI agent context file. Describes project purpose, repo layout, build/test commands, architecture rules, and coding conventions so agents can operate effectively without additional prompting.

---

## Project Context

**Excalidraw** is an open-source virtual whiteboard with a hand-drawn aesthetic.  
Live app: https://excalidraw.com  
npm package: `@excalidraw/excalidraw`

This monorepo contains:
- The web application deployed to excalidraw.com (`excalidraw-app/`)
- The embeddable React component package (`packages/excalidraw/`)
- Supporting internal packages (`common`, `math`, `element`, `utils`)
- Docusaurus documentation site (`dev-docs/`)
- Integration examples (`examples/`)

**Core stack:** TypeScript · React 19 · Vite · Yarn workspaces · Vitest · ESLint · Prettier

---

## Repo Structure

```
excalidraw-app/          App shell: entry point, collab, persistence, menus, sharing
packages/
  excalidraw/            Public React component package (@excalidraw/excalidraw)
  element/               Element model: bounds, transforms, selection, mutation
  math/                  2D geometry primitives (Point, Vector, curves, …)
  common/                Shared constants, utils, event bus, app state helpers
  utils/                 Lower-level helpers (file I/O, compression, freehand)
dev-docs/                Docusaurus documentation website
examples/
  with-nextjs/           Next.js integration example
  with-script-in-browser/ Vite/browser integration example
scripts/                 Build, release, locale, and wasm tooling
firebase-project/        Firebase security rules and config
public/                  Static assets served by the app
```

---

## Commands

Run from the repository root unless noted.

### Setup

```bash
yarn                        # install all workspace dependencies
```

Requirements: Node.js >= 18, Yarn.

### Development

```bash
yarn start                  # start excalidraw-app dev server (http://localhost:3000)
yarn start:production       # build then serve production bundle locally
yarn start:example          # build packages then start browser example
```

### Build

```bash
yarn build:packages         # build common + math + element + excalidraw packages
yarn build                  # build excalidraw-app (includes version metadata)
yarn build:preview          # build app and run vite preview
yarn build:app:docker       # build app without analytics/tracking (Docker target)
```

### Test and Quality

```bash
yarn test                   # run app tests (Vitest, watch mode)
yarn test:app               # same as above
yarn test:all               # typecheck + lint + format check + tests (no watch)
yarn test:typecheck         # tsc type check
yarn test:code              # ESLint (max 0 warnings)
yarn test:other             # Prettier format check
yarn test:update            # update snapshots
yarn test:coverage          # run tests with v8 coverage
yarn fix                    # auto-fix formatting and lint issues
yarn fix:code               # ESLint fix only
yarn fix:other              # Prettier write only
```

### Docs

```bash
yarn --cwd dev-docs start   # start docs site (http://localhost:3003)
yarn --cwd dev-docs build   # build static docs
```

### Cleanup

```bash
yarn rm:build               # remove all dist/build artifacts
yarn rm:node_modules        # remove all node_modules
yarn clean-install          # rm:node_modules + yarn install
```

### Docker

```bash
docker-compose up --build -d   # run app in Docker (port 3000)
docker build -t excalidraw/excalidraw .
docker run --rm -dit --name excalidraw -p 5000:80 excalidraw/excalidraw:latest
```

---

## Architecture Rules

### Dependency Direction

Packages must only depend on packages below them in this chain:

```
@excalidraw/common
  └── @excalidraw/math
        └── @excalidraw/element
              └── @excalidraw/excalidraw
                    └── excalidraw-app
```

**Do not introduce reverse dependencies.**

### Placement Rules

| Logic type | Where it belongs |
|---|---|
| Reusable editor/domain logic | `packages/*` |
| App wiring, persistence, collab | `excalidraw-app/*` |
| Behavior needed by app AND embed consumers | `packages/excalidraw` or lower |
| App-only UI (menus, sharing, banners) | `excalidraw-app/components/` |

### Imports

- Use public entry points (`@excalidraw/element`, `@excalidraw/common`, etc.).
- Do not reach into internal files of another package.
- If architecture is affected by a change, update `dev-docs/` accordingly.

---

## Coding Conventions

### TypeScript

- All new code in TypeScript (`.ts` / `.tsx`).
- Prefer `const` and `readonly`; avoid unnecessary mutation.
- Use `?.` and `??`; avoid verbose null guard blocks.
- Prefer implementations without allocation in hot paths; trade RAM for fewer CPU cycles where sensible.

### React

- Functional components with hooks only — no class components.
- Follow Rules of Hooks strictly (no conditional hooks, stable call order).
- Keep components small and single-responsibility.
- Use CSS modules for new component styles.

### Naming

| Kind | Convention |
|---|---|
| Components, interfaces, type aliases | `PascalCase` |
| Variables, functions, methods | `camelCase` |
| Constants | `ALL_CAPS` |

### Error Handling

- Wrap async operations in `try/catch` when failures are expected or recoverable.
- Add React error boundaries for UI surfaces that render external/async content.
- Log errors with contextual information (operation, relevant IDs/state).
- Never silently swallow errors with empty `catch` blocks.

### Math / Geometry

- Always use `Point` from `packages/math/src/types.ts` for 2D coordinates.
- Do not use ad-hoc `{ x, y }` object types in math APIs.

---

## Key Entry Points

| File | Purpose |
|---|---|
| `excalidraw-app/index.tsx` | App boot, React root, service worker |
| `excalidraw-app/App.tsx` | Top-level app composition |
| `excalidraw-app/collab/Collab.tsx` | Real-time collaboration integration |
| `excalidraw-app/data/` | Persistence, Firebase, localStorage, file lifecycle |
| `packages/excalidraw/index.tsx` | Public React component + providers |
| `packages/excalidraw/types.ts` | Public API types |
| `packages/element/src/index.ts` | Element operations public API |
| `packages/math/src/index.ts` | Math primitives public API |
| `packages/common/src/index.ts` | Shared constants/utilities public API |

---

## Testing Guidance

- Prefer targeted checks (`yarn test:typecheck`, `yarn test:code`) before running full suite.
- Always offer to run `yarn test:app` after code edits.
- Fix reported failures before marking work done.
- Do not leave empty or skipped tests without explanation.

---

## External Links

- Docs: https://docs.excalidraw.com
- npm: https://www.npmjs.com/package/@excalidraw/excalidraw
- Discord: https://discord.gg/UexuTaE
- Issues: https://github.com/excalidraw/excalidraw/issues
