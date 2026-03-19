# Phase 1 Brief — Foundation Spine

## Goal
Stand up the repo with a working Tauri + React app shell, monorepo structure, command registry skeleton, shared domain types, basic Zustand store architecture, test harness, and architecture docs.

At the end of Phase 1, the app should launch, show a workspace layout with panel placeholders, and have a working command bus that routes commands from UI to Rust backend.

## Deliverables
1. Tauri v2 + React monorepo scaffold (pnpm workspaces)
2. Workspace panel layout (sidebar, main area, inspector, bottom panel)
3. Rust command registry skeleton with one example command
4. Shared domain types package (`@composecraft/domain`)
5. Zustand store architecture (`@composecraft/state`)
6. Test harness (vitest for TS, cargo test for Rust)
7. Architecture docs spine

## Monorepo Structure
```
composecraft/
  apps/
    desktop/
      src/              # React frontend
      src-tauri/        # Rust backend
  packages/
    domain/             # Shared TypeScript types
    state/              # Zustand stores
  docs/
    architecture.md
  package.json          # pnpm workspace root
  pnpm-workspace.yaml
```

## Packet Graph

### Wave 1: Scaffold (parallel)
- **p1-scaffold-monorepo** (architect) — Create pnpm monorepo, Tauri v2 scaffold, workspace config, tsconfig, basic package.json files. This is the foundation everything else builds on.

### Wave 2: Domain + Backend + State (parallel, depends on scaffold)
- **p1-domain-types** (builder) — Create `@composecraft/domain` with core types: Project, Asset, Layer, Command, CommandResult, Panel, WorkspaceLayout
- **p1-backend-commands** (builder) — Create Rust command module skeleton in src-tauri: command registry pattern, one example `ping` command, structured error type
- **p1-state-stores** (builder) — Create `@composecraft/state` with Zustand stores: workspaceStore (panels, layout), projectStore (project metadata), commandStore (command history)

### Wave 3: UI Shell (depends on state + domain)
- **p1-ui-shell** (builder) — Create React app shell with workspace layout: sidebar panel, main canvas area, inspector panel, bottom panel (logs/jobs). Wire to workspaceStore for panel visibility.

### Wave 4: Integration + Docs (depends on all above)
- **p1-integration-wiring** (integrator) — Register commands in lib.rs, wire store exports, connect UI shell to command bus, verify app launches
- **p1-docs-architecture** (builder) — Write architecture.md documenting: monorepo structure, command flow, state architecture, panel system, protected/seam files

## Protected Files
- `apps/desktop/src-tauri/src/lib.rs`
- `packages/domain/src/index.ts`
- `packages/state/src/index.ts`
- `apps/desktop/src/App.tsx`

## Seam Files
- Same as protected (integration packet owns these)

## Role Assignments
| Packet | Role | Model |
|--------|------|-------|
| p1-scaffold-monorepo | architect | opus |
| p1-domain-types | builder | sonnet |
| p1-backend-commands | builder | sonnet |
| p1-state-stores | builder | sonnet |
| p1-ui-shell | builder | sonnet |
| p1-integration-wiring | integrator | opus |
| p1-docs-architecture | builder | sonnet |

## Human Gates
- Phase 1 feature approval (before execution)
- Merge approval (after all packets verified)

## Success Criteria
- App launches via `cargo tauri dev`
- Workspace layout renders with 4 panel areas
- `ping` command round-trips from UI to Rust and back
- All tests pass
- Architecture doc covers the real structure
