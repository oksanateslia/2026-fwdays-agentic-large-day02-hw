---
name: review-code
description: "Structured code review for Excalidraw changes: correctness, safety, tests, and performance."
---
# Command: /review-code

## When to use
Use after changes are proposed and you need a repeatable, high-signal review of diffs.

## Inputs
- Diff or list of changed files
- Review depth: quick | deep (default: quick)

## Steps
1. Scope the changes
   - Identify affected packages, entry points, and shared utilities.
   - Note any API or behavior changes.
2. Correctness and regressions
   - Check logic for edge cases, null/undefined paths, and async flows.
   - Verify TypeScript strictness: no `any`, no `@ts-ignore`.
3. React and UI concerns (if applicable)
   - Hooks usage is correct (no conditional hooks).
   - Component props are typed as `{ComponentName}Props`.
4. Performance and memory
   - Prefer fewer allocations where possible.
   - Verify no unnecessary renders or expensive loops in hot paths.
5. Security and data handling
   - Validate user input before use or persistence.
   - Avoid leaking sensitive data in logs or telemetry.
6. Tests
   - Confirm new/changed behavior has tests or a clear justification.
   - If tests are updated, ensure deterministic behavior and clear assertions.
7. Output findings
   - List issues by severity with file/line references.
   - Provide suggested fixes or follow-up questions.

## Expected output
- A list of findings ordered by severity with file/line references.
- Open questions or assumptions that need confirmation.
- Suggested verification commands when relevant (e.g., `yarn test:code`, `yarn test:typecheck`).

## Example
User: /review-code depth=deep files=packages/excalidraw/src/*
Assistant:
- High: `packages/excalidraw/src/foo.ts` line 42 — possible null deref on `scene?.elements`.
- Medium: `packages/excalidraw/src/bar.tsx` line 118 — new prop missing `{ComponentName}Props` type.
- Low: `packages/excalidraw/src/baz.ts` line 12 — extra array allocation in tight loop; prefer indexed loop.
Questions:
- Is `FooStore` ever undefined during `loadScene`?
Verification:
- `yarn test:code`
- `yarn test:typecheck`

