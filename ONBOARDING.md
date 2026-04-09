# Excalidraw Monorepo Onboarding

## 1. Project Description

This repository is the Excalidraw monorepo. It contains:

- The web app used on excalidraw.com (workspace: excalidraw-app)
- The embeddable React package published as @excalidraw/excalidraw
- Supporting internal/public packages for shared logic (common, math, element, utils)
- Development docs (Docusaurus) and integration examples

Core stack:

- TypeScript + React
- Vite for app/dev bundling
- Yarn workspaces (monorepo)
- Vitest + ESLint + Prettier for quality gates

## 2. Repository Architecture

## 2.1 Monorepo Layout

- excalidraw-app/: production app shell, app-specific state, persistence, and collaboration integration
- packages/excalidraw/: main React component package (public API)
- packages/common/: shared constants/utilities used across packages
- packages/math/: geometry and 2D math helpers
- packages/element/: element model logic, transforms, bounds, selection helpers
- packages/utils/: lower-level runtime helpers used by Excalidraw packages
- dev-docs/: documentation website (Docusaurus)
- examples/: integration examples (Next.js and script-in-browser)
- scripts/: build and release scripts for packages/app/docs/locales/wasm assets

## 2.2 Runtime Architecture (High Level)

1. Entry point
   - excalidraw-app/index.tsx boots React, registers service worker, and renders the app.

2. App shell
   - excalidraw-app/App.tsx composes the top-level UX for excalidraw.com:
     - editor embedding
     - menus/sidebar/welcome screen
     - local persistence and file lifecycle
     - collaboration hooks and sharing flows

3. Editor package
   - packages/excalidraw/index.tsx exposes the Excalidraw React component and providers.
   - packages/excalidraw/* contains rendering, interaction, app state, data restore/export, and UI components.

4. Domain packages
   - packages/element handles scene element operations.
   - packages/math provides geometric primitives and operations.
   - packages/common centralizes shared constants/utils.
   - packages/utils contains extra reusable helpers used by package code.

5. External systems
   - Collaboration is wired from app code and depends on a separate collab server setup.
   - Firebase-related modules in excalidraw-app/data support storage/file workflows.

## 2.3 Build and Distribution Model

- Root workspace orchestrates builds/tests/linting.
- Package builds output dist/ artifacts (dev/prod bundles + types).
- App build produces static output for deployment.
- Dockerfile/docker-compose are available for containerized local runs and self-hosting.

## 3. Main Commands

Run from repository root unless noted.

## 3.1 Setup

```bash
yarn
```

Requirements:

- Node.js >= 18
- Yarn (workspace uses Yarn classic style lockfile/scripts)

## 3.2 Daily Development

```bash
yarn start
```

Starts excalidraw-app (Vite dev server, default local app port 3000).

```bash
yarn start:production
```

Builds then serves production build locally.

## 3.3 Build

```bash
yarn build:packages
```

Builds shared packages (common, math, element, excalidraw).

```bash
yarn build
```

Builds excalidraw-app (plus version metadata).

```bash
yarn build:preview
```

Builds app and runs Vite preview.

## 3.4 Tests and Quality

```bash
yarn test
```

Runs app tests (Vitest).

```bash
yarn test:all
```

Runs typecheck + lint + formatting checks + app tests.

```bash
yarn test:typecheck
yarn test:code
yarn test:other
```

Targeted checks for TypeScript, ESLint, and Prettier.

```bash
yarn fix
```

Auto-fixes lint and formatting issues where possible.

```bash
yarn test:update
```

Updates snapshots.

## 3.5 Documentation and Examples

Docs site (from dev-docs/):

```bash
yarn --cwd dev-docs start
yarn --cwd dev-docs build
```

Examples:

```bash
yarn --cwd examples/with-nextjs dev
yarn --cwd examples/with-script-in-browser start
```

## 3.6 Utility Commands

```bash
yarn rm:build
```

Removes generated build artifacts.

```bash
yarn clean-install
```

Removes node_modules and reinstalls dependencies.

## 3.7 Docker (Optional)

```bash
docker-compose up --build -d
```

Runs the app via Docker Compose.

## 4. Suggested First-Day Path

1. Install dependencies with yarn.
2. Run yarn start and verify local app boot.
3. Read excalidraw-app/App.tsx to understand top-level app composition.
4. Read packages/excalidraw/index.tsx to understand public component entry.
5. Run yarn test:all before opening your first PR.

## 5. Contributor Notes

- The repository includes both product app code and publishable packages.
- Prefer fixing issues in package layers when behavior is shared across app and integrations.
- Use dev-docs for deeper API and contribution guidance.