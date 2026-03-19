# ComposeCraft — Product Constitution

## Thesis
ComposeCraft is a desktop composition workspace where users create projects, manage layered assets, trigger background operations, inspect state, undo/redo changes, save/load project files, and export results.

## Purpose
This repo exists to pressure-test the multi-claude build system under real architectural complexity. Product quality matters, but system truth matters more.

## Architecture
- **Runtime:** Tauri v2 + React + TypeScript
- **Backend:** Rust (Tauri commands)
- **State:** Zustand stores (TypeScript)
- **Domain types:** Shared TypeScript package
- **Persistence:** SQLite via Rust
- **UI:** React components with workspace panel layout

## Non-Negotiable Rules
1. All mutations go through the command system — no direct state writes from UI
2. Undo/redo is a first-class system, not an afterthought
3. Every command is registered, not ad-hoc
4. Project files have a schema version field from day one
5. Background jobs are cancellable and report progress
6. Tests ship with the code that needs them

## Architecture Spine (protected files)
These files define boundaries and may only be changed through explicit contract packets:
- `apps/desktop/src-tauri/src/lib.rs` — command registration
- `packages/domain/src/index.ts` — domain type exports
- `packages/state/src/index.ts` — store exports
- `apps/desktop/src/App.tsx` — root layout/routing

## Seam Files (integrator-owned)
- `apps/desktop/src-tauri/src/lib.rs` — command registry
- `packages/state/src/index.ts` — store barrel
- `packages/domain/src/index.ts` — type barrel
- `apps/desktop/src/App.tsx` — panel wiring

## Phase Plan
- Phase 1: Foundation spine (scaffold, layout, registries, test harness)
- Phase 2: Document model + persistence
- Phase 3: Command/action system with undo/redo
- Phase 4: Asset/layer workspace
- Phase 5: Background jobs + progress
- Phase 6: Export/import
- Phase 7: Hardening + release
