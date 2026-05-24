# AGENTS.md

Guidance for AI agents working in the `space-together-desktop` repository.

## Project Overview

Space Together is a school collaboration and management system for students,
teachers, parents, school staff, and administrators. This repository is the
desktop client: a Next.js frontend packaged with Tauri.

Sibling repositories in the same workspace:

- `space-together-api`: Rust/Actix backend API and OpenAPI documentation.
- `space-together-web`: Next.js web client.
- `space-together-platform`: Next.js platform/admin-style client.
- `space-together-desktop`: Next.js client wrapped by Tauri.

Default Next.js dev port: `4747`.
Default production start port: `4748`.
Backend API default port: `4646`.
Tauri config: `src-tauri/tauri.conf.json`.

## Repository Structure

- `src/app`: Next.js App Router pages, layouts, and global styles.
- `src/components`: Reusable UI and feature components.
- `src/service`: API client wrappers and domain service modules.
- `src/lib`: Shared utilities, schemas, hooks, contexts, constants, crypto, and
  messaging helpers.
- `src/locale`: Translation dictionaries for `en` and `rw`.
- `src/router.ts`: Public/auth route lists and default redirects.
- `src/proxy.ts`: Locale and auth-aware request proxy logic.
- `src-tauri`: Tauri shell, Rust crate, capabilities, icons, and packaging
  configuration.
- `biome.json`: Formatting and lint rules.

## Frontend And Desktop Patterns

- Use the App Router structure under `src/app`.
- Preserve locale-aware routes. The app supports `en` and `rw`; user-facing text
  that belongs in translations should be reflected in both dictionaries when
  practical.
- Use existing service modules in `src/service` for backend calls. Shared HTTP
  behavior lives in `src/service/api-client.ts`.
- Respect auth helpers and cookies in `src/lib/utils/auth-context.ts` and
  related files.
- Prefer existing UI components and local patterns before creating new
  primitives.
- Keep forms aligned with existing `react-hook-form` and Zod schema patterns.
- Use Biome formatting style: 2-space indentation, double quotes, semicolons.
- For desktop behavior, check `src-tauri/tauri.conf.json`,
  `src-tauri/src/lib.rs`, and `src-tauri/capabilities/default.json`.
- Do not change Tauri permissions, identifiers, windows, or bundle settings
  casually; verify why the change is needed.

## API Contract Rule

If a change requires a new backend endpoint or changes an existing backend
contract, update the API repository too:

- Code: `../space-together-api`
- OpenAPI file: `../space-together-api/docs/openapi.json`

Any new or changed public API must be documented in that OpenAPI file in the
same task and committed in the API repository. Keep frontend service types and
OpenAPI schemas in sync.

## Development Commands

- Install dependencies: `bun install`
- Run Next.js locally: `bun run dev`
- Build Next.js: `bun run build`
- Lint/check: `bun run lint`
- Format: `bun run format`
- Run Tauri dev flow when needed: `bun tauri dev`
- Build desktop app when needed: `bun tauri build`

Use the narrowest verification that proves the change. For desktop-specific
changes, verify both the frontend behavior and the Tauri setting or Rust code
that changed.

## Git Rules

- Every AI-made file change, even a very small one, must be committed before the
  agent gives its final answer.
- Stage only the files changed for the current task. Do not stage unrelated
  user edits.
- If a task touches multiple sibling repositories, make a separate commit in
  each affected repository.
- Before committing, run `git status --short` and check the diff.
- Use short, clear commit messages, for example:
  `docs: add agent guidance`
  `feat: add desktop notification setting`
  `fix: keep tauri dev url aligned`
- Never rewrite history, reset, or discard user changes unless the user clearly
  asks for that exact operation.

## Working Style

- Read nearby components, services, and Tauri files before editing.
- Keep UI work consistent with the existing design system and component
  library.
- Avoid broad refactors unless needed for the requested change.
- Keep secrets out of commits. Do not commit `.env` values or credentials.
- Use ASCII in new files unless the surrounding file already needs Unicode.

