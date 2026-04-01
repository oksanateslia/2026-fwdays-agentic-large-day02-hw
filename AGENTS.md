# AGENTS.md

## Project Overview

Excalidraw is an open source, hand-drawn style whiteboard that ships as both a hosted app and an npm package. The repository includes the core editor library and the full excalidraw.com app, plus shared packages and integration examples. Collaboration and end-to-end encryption are core capabilities of the app and influence design decisions across the codebase.

## Project Structure

Excalidraw is a **monorepo** with a clear separation between the core library and the application:

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** - Core packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** - Integration examples (NextJS, browser script)

## Tech Stack

- TypeScript (strict)
- React (component library + app)
- Vite (app build)
- esbuild (package build)
- Yarn workspaces (monorepo)

## Development Workflow

1. **Package Development**: Work in `packages/*` for editor features
2. **App Development**: Work in `excalidraw-app/` for app-specific features
3. **Testing**: Always run `yarn test:update` before committing
4. **Type Safety**: Use `yarn test:typecheck` to verify TypeScript

## Development Commands


<CodeBlockWrapper v-bind="{}" :ranges='[]'>

```bash
yarn test:typecheck  # TypeScript type checking
yarn test:update     # Run all tests (with snapshot updates)
yarn fix             # Auto-fix formatting and linting issues
```

</CodeBlockWrapper>

## Conventions

- Naming: kebab-case files, PascalCase components, `{ComponentName}Props` for props types.
- Code style: functional components + hooks only, named exports only, strict TypeScript with typed imports.
- PR workflow: follow the contributing docs and keep the test/typecheck steps green before requesting review.

## Do-Not-Touch / Constraints

- Protected files in `packages/excalidraw/` (renderer, restore, action manager, core types) require explicit approval and full test/QA coverage.
- Architecture constraints: actionManager-driven state updates, Canvas 2D render pipeline, and no new dependencies without approval.

## Cursor Rules

- [architecture.mdc](.cursor/rules/architecture.mdc) — actionManager-driven state, Canvas 2D rendering, and dependency approvals.
- [assets.mdc](.cursor/rules/assets.mdc) — asset locations per consumer (public, docs, fonts, .github assets).
- [conventions.mdc](.cursor/rules/conventions.mdc) — component/TypeScript conventions and naming rules.
- [do-not-touch.mdc](.cursor/rules/do-not-touch.mdc) — protected files and required approvals/QA.
- [security.mdc](.cursor/rules/security.mdc) — input validation, safe defaults, SVG handling, and dependency checks.
- [testing.mdc](.cursor/rules/testing.mdc) — deterministic tests, snapshot discipline, and verification commands.

## Architecture Notes

### Package System

- Uses Yarn workspaces for monorepo management
- Internal packages use path aliases (see `vitest.config.mts`)
- Build system uses esbuild for packages, Vite for the app
- TypeScript throughout with strict configuration
