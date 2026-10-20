---
title: Runtime View
order: 6
---

### 6.1 Startup

1. `run()` migrates any plaintext credentials from `profiles.json` into secure storage.
2. Crash-report consent is read from settings and mirrored into the panic hook's flag.
3. Plugins and the native menu are registered; the Aptabase plugin is a no-op when no app key was compiled in or consent is absent.
4. The webview loads; stores hydrate (`settings`, `server` → active profile). The updater store performs a startup check.

### 6.2 Browse an EHR's compositions

![Runtime view: browse an EHR's compositions](../images/06-runtime-browse.png)

### 6.3 Create a composition from a template

1. User opens `/compose/:templateId/:ehrId`; `template` store fetches the Web Template.
2. `CompositionForm` renders the template with medblocks-ui (loaded from CDN with SRI).
3. On submit the `composition` store invokes the create command; Rust POSTs the composition and parses the new UID from the `Location` header ([ADR-0009](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0009-location-header-parsing-for-resource-creation.md)).
4. Store navigates to the viewer; errors surface via the three-layer strategy.

### 6.4 Run an AQL query

`AqlRunner` → `query` store → `execute_aql` command → CDR `/query/aql` → rows/columns
returned and rendered as a (virtualized) table; CSV export is client-side; saved
queries are persisted by Rust. Autocomplete combines keyword, template-path, and
archetype-aware suggestions in the `aql/` library.

### 6.5 Save a server profile

`ServerManager` → `server` store → server-profile save command. Rust splits the profile:
non-secret fields go to `profiles.json`; password/token go to `CredentialStore`
(keychain, else encrypted file). Frontend receives profiles without secrets.

### 6.6 Update check

Startup or menu "Check for Updates…" (`check-for-updates-requested` event) → `update`
store → Tauri updater fetches `latest.json` from GitHub Releases, verifies the
minisign signature, downloads, installs, restarts ([ADR-0017](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0017-tauri-updater-with-github-releases.md)).
