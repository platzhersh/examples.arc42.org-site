---
title: Context and Scope
order: 3
---

### 3.1 Business Context

![Business context](../images/03-business-context.png)

| Partner | Inputs / Outputs |
|---|---|
| **User** | Interactive UI; provides server URL and credentials, AQL, form data |
| **openEHR CDR** | Receives REST calls (EHR, composition, template, query, contribution, directory); returns JSON/XML. Responses are untrusted input |
| **GitHub Releases** | Serves `latest.json` updater manifest and signed installers |
| **Aptabase** | Receives anonymous, opt-in usage events (no PII) — [ADR-0018](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0018-aptabase-opt-in-usage-analytics.md) |
| **Website** | Separate Astro + Vue static site, deployed via GitHub Pages — [ADR-0024](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0024-astro-vue-marketing-site.md) |

### 3.2 Technical Context

| Channel | Protocol | Notes |
|---|---|---|
| Webview ⇄ Rust | Tauri IPC (`invoke`) and Tauri events | Typed command signatures; `cdr-inspector-entry`, `check-for-updates-requested` events |
| Rust → CDR | HTTP(S) via `reqwest`; Basic/Bearer/none auth | Base path from `profile.api_root()`; vendor admin endpoints off `base_url` |
| Rust → OS | Keychain (`keyring`), config dir files, native menu, dialogs | Encrypted-file fallback where no keychain exists |
| Webview → CDN | HTTPS (unpkg, jsDelivr) | medblocks-ui and a few assets, with SRI ([ADR-0008](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0008-cdn-based-medblocks-ui.md), [ADR-0016](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0016-csp-and-subresource-integrity.md)) |
| App → GitHub | HTTPS | Tauri updater, minisign-verified |

**Out of scope:** the application does not persist or cache clinical data, does
not implement a CDR, and does not provide multi-user features.
