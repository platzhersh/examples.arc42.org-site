---
title: Building Block View
order: 5
---

### 5.1 Level 1 — Whitebox: openEHR Explorer

![Building block view, level 1](../images/05-building-block-level1.png)

| Building block | Responsibility |
|---|---|
| **Frontend (webview)** | Presentation, navigation, client state; no direct CDR access |
| **Rust core** | CDR communication, persistence, secrets, native integration, instrumentation |
| **Website** (`website/`) | Static marketing + documentation site; shares the Storybook bundle ([ADR-0026](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0026-shared-storybook-app-and-website.md)); not part of the shipped app |
| **CI/CD & packaging** (`.github/workflows`, `packaging/`, `scripts/`) | Build, sign, publish, and update-manifest pipelines |

### 5.2 Level 2 — Rust Core

| Module | Responsibility |
|---|---|
| `lib.rs` | `run()`: credential migration, crash-report consent seeding, plugin registration (opener, http, dialog, fs, updater, process, aptabase), native menu, `generate_handler![]` command registry |
| `commands/server.rs` | Profile CRUD, connection test, server-type/version detection, plaintext-credential migration. Owns `ServerProfile`, `ServerType`, `AuthMethod`, `api_root()` |
| `commands/ehr.rs` | EHR list/detail/create/delete/search (AQL-based per ADR-0010/0027) |
| `commands/composition.rs` | Composition get (structured, FLAT, versions), create, update, delete |
| `commands/template.rs` | Template list, Web Template fetch, OPT XML get/upload |
| `commands/query.rs` | AQL execution, saved-query storage |
| `commands/terminology.rs` | Code lookups with `TerminologyCache` |
| `commands/contribution.rs` | Contribution audit-trail retrieval ([ADR-0020](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0020-contribution-audit-trail-view.md)) |
| `commands/dashboard.rs` | Aggregated counts and server info for the landing dashboard |
| `inspector.rs` | `send_instrumented()`: capture, redact, cap (2 MB), emit `cdr-inspector-entry` |
| `credentials.rs` | `CredentialStore`: keychain primary, AES-GCM encrypted file fallback, file-permission hardening, zeroization |
| `settings.rs` | Persisted app settings (analytics consent, etc.) |
| `crash_report.rs` | Consent-gated panic hook |

**Interface to the frontend:** `#[tauri::command] async fn(...) -> Result<T, String>`.
Every command that talks to a CDR receives the `AppHandle` so it can call
`send_instrumented`.

### 5.3 Level 2 — Frontend

| Area | Contents |
|---|---|
| **Views** (`src/views`) | `Dashboard`, `EhrBrowser`, `CompositionViewer`, `CompositionForm`, `ContributionViewer`, `TemplateBrowser`, `AqlRunner`, `TerminologyBrowser`, `ServerManager`, `Settings` |
| **Routing** (`src/main.ts`) | `/dashboard`, `/ehrs[/:ehrId[/compositions/:uid \| /contributions/:uid]]`, `/templates[/:id]`, `/aql`, `/terminology`, `/servers`, `/settings`, `/compose/:templateId[/:ehrId]`, `/edit/:ehrId/:uid` |
| **Stores** (`src/stores`) | `server`, `ehr`, `composition`, `contribution`, `template`, `query`, `dashboard`, `settings`, `inspector`, `update`, `tour`, `whatsNew` — one per domain ([ADR-0004](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0004-separate-pinia-stores-per-domain.md)) |
| **Components** (`src/components`) | Shared UI: `AppSidebar`, `RequestInspector`, `CompositionTree`, `JsonViewer`/`XmlViewer`/`BashViewer` ([ADR-0021](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0021-harmonized-json-viewer-component.md), [0022](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0022-harmonized-xml-viewer-component.md)), `AqlEditor` (CodeMirror, [ADR-0012](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0012-codemirror-for-aql-editor.md)), `DirectoryTree`, icons, buttons; each with a Storybook story |
| **Libraries** (`src/lib`) | Pure logic: `webtemplate.ts` (tree traversal, FLAT paths), `aql/` (autocomplete), `terminology.ts`, `apiPath.ts`, `virtualList.ts`, `xml.ts`, `tours.ts`, `whats-new.ts`, `errorReport.ts`, `navShortcuts.ts` |

**Rule of dependency:** views → stores/components/lib; stores → Tauri `invoke`/events;
components never call `invoke` directly (they receive props/emit events), which
keeps them Storybook-renderable with `storybook-tauri.ts` mocks.

### 5.4 Level 3 — Request Instrumentation (`inspector.rs`)

![Request instrumentation, level 3](../images/05-building-block-level3-inspector.png)
