# Initial Scaffolding Notes

This document captures the baseline structure that was generated when the Finance Hub workspace was first scaffolded. Use it as a quick refresher whenever you need to recreate the project or explain its layout to new collaborators.

## Workspace Overview
- **Monorepo tooling**: Nx v22 powers the workspace, enabling consistent linting, testing, and builds across all projects.
- **Package manager**: npm (Node.js v20+) manages dependencies declared in the root `package.json`.
- **Primary apps**:
  - `backend/`: NestJS service that will expose the API surface and background jobs.
  - `frontend/`: React + Vite client that consumes the backend APIs.
- **Shared config**: Root-level ESLint, Jest, and TypeScript configs are extended by each app to avoid duplication.

```
finance-hub/
├─ backend/        # NestJS app + Prisma schema
├─ frontend/       # React (Vite) single-page app
├─ docs/           # Project documentation (this file lives here)
├─ nx.json         # Nx task graph + project configuration
├─ package.json    # Root dependencies and scripts
└─ tsconfig.base.json
```

## Generation Steps
- Initialize the workspace: `npx create-nx-workspace@latest finance-hub --preset=apps --nxCloud=false`
- Generate the backend service: `npx nx g @nx/nest:app backend`
- Generate the frontend client: `npx nx g @nx/react:app frontend --bundler=vite`
- Add testing presets via Nx plugins (`@nx/jest`, `@nx/vitest`) for server and client respectively.

> ⚠️ If any of the generators change defaults in later Nx releases, re-run them in a scratch repo first to diff the outputs before altering this workspace.

## Backend Stack
- **Framework**: NestJS 10 with Express adapter.
- **ORM**: Prisma (schema stored under `backend/prisma/schema.prisma`).
- **Tooling**: SWC for compilation, Jest for unit tests, ESLint for linting.
- **Entry point**: `backend/src/main.ts` bootstraps the HTTP server.

## Frontend Stack
- **Framework**: React 18 rendered via Vite 7 dev server/build pipeline.
- **UI toolkit**: Mantine (core + hooks + dates) is preinstalled for component primitives.
- **Testing**: Vitest + Testing Library for component/unit specs.
- **Entry point**: `frontend/src/main.tsx` mounts the root application shell.

## Dev Commands Cheat Sheet
- `npm install` — install all workspace dependencies.
- `npx nx serve backend` — start the NestJS API locally on port 3000.
- `npx nx serve frontend` — run the React dev server (default port 4200 under Nx).
- `npx nx test backend` / `npx nx test frontend` — execute server/client test suites.
- `npx nx run backend:prisma-migrate` — apply Prisma migrations to the local database.
