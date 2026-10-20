---
title: Architecture Constraints
order: 2
---

### 2.1 Technical

| Constraint | Background |
|---|---|
| **Tauri 2** (Rust backend + system webview) | Small native footprint, OS integration (keychain, updater, menus); see [PRD-0001](https://github.com/platzhersh/openehr-explorer/blob/main/docs/prd/PRD-0001-openehr-explorer-desktop-cdr-browser.md) |
| **Vue 3 + TypeScript + Pinia** frontend | Composition API, `<script setup>`; Vite as build tool |
| **All CDR HTTP traffic goes through Rust** (`reqwest`) | Avoids webview CORS limits, enables central instrumentation and credential isolation ([ADR-0011](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0011-centralized-request-instrumentation.md)) |
| **openEHR REST API** as the integration contract | Vendor deviations handled per `server_type` |
| **Pinned dependency versions** | Reproducible builds ([ADR-0007](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0007-pinned-dependency-versions.md)) |
| **No bundled backend service** | Application is a pure client; it owns no server-side state |

### 2.2 Organizational

| Constraint | Background |
|---|---|
| Open source, Apache-2.0 | Public GitHub repo `platzhersh/openehr-explorer` |
| PRD/ADR governance | [ADR-0001](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0001-use-prd-and-adr-documentation.md) |
| Conventional Commits, PR-based workflow | See `CLAUDE.md` / `CONTRIBUTING.md` |
| Tag-triggered releases | See [`RELEASING.md`](https://github.com/platzhersh/openehr-explorer/blob/main/RELEASING.md) |

### 2.3 Conventions

- Lint/format: `oxlint` / `oxfmt` (frontend), `cargo fmt` / `cargo clippy -D warnings` (Rust).
- Type check with `vue-tsc`; unit tests with Vitest (frontend) and `cargo test` (backend).
- Components documented in Storybook ([ADR-0021](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0021-storybook-for-component-development.md)); WCAG AA colour contrast enforced ([ADR-0028](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0028-color-contrast-gate.md)).
- No text-glyph carets; use `CaretIcon` / `CollapsibleSection` (enforced by test).
