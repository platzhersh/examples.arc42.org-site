---
title: Cross-cutting Concepts
order: 8
---

### 8.1 Security

- **Credentials:** keychain-first storage, encrypted-file fallback, zeroization, never returned to the frontend, `Authorization`/`Cookie` redacted in the Inspector ([ADR-0015](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0015-secure-credential-storage.md), [ADR-0011](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0011-centralized-request-instrumentation.md)).
- **Hostile server data:** treat CDR responses as untrusted; escape on render; validate user input ([ADR-0014](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0014-xss-hardening-and-input-validation.md)).
- **CSP + SRI:** CSP in `tauri.conf.json` limits script/style sources; CDN assets pinned with SRI ([ADR-0016](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0016-csp-and-subresource-integrity.md)). `connect-src` permits arbitrary `http(s)` because users add arbitrary CDR URLs. CDR traffic is made from Rust, but the broad directive leaves a residual risk that compromised webview code could send requests to arbitrary destinations (see R3).
- **Supply chain:** pinned dependencies + committed lockfiles, signed updater artifacts, code-signed Windows installer.
- **Privacy:** analytics and crash reporting are opt-in, contain no PII ([ADR-0018](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0018-aptabase-opt-in-usage-analytics.md)).

### 8.2 Observability: Request Inspector

All CDR traffic is observable via the event pipeline in §5.4. Any new CDR call
**must** use `send_instrumented` rather than calling `.send()` directly.

### 8.3 Vendor abstraction

`ServerProfile.server_type` (`ehrbase | better_platform | ferro_ehr | generic`) and
`api_path_prefix` isolate vendor differences. Rules:

- Build openEHR URLs from `profile.api_root()`; never hardcode `/rest/openehr/v1`.
- Vendor-specific non-openEHR endpoints (e.g. EHRBase `/rest/admin`, `/rest/status`) hang off `base_url`.
- Where the spec lacks an endpoint (EHR listing), use AQL ([ADR-0010](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0010-aql-based-listing-for-ehr-and-composition.md)); know its limits ([ADR-0027](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0027-ehr-level-attribute-aql-limitations.md)).
- Version detection informs feature availability (PRD-0014).

### 8.4 Error handling

Three layers ([ADR-0005](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0005-three-layer-error-handling.md)): Rust commands return contextual `Err(String)`;
stores catch, store `error` state, and map to user-friendly text; views render
`DismissibleError`. Unexpected frontend errors are reported only with analytics consent, via `errorReport.ts`, which sends a bounded error class and scrubbed `file:line` — never messages (they may contain URLs, EHR IDs or AQL).

### 8.5 State management and persistence

Pinia (composition style) per domain; the Rust side is the single source of truth
for persisted state (profiles, settings, saved queries). The frontend uses `localStorage` only for minor UI conveniences (e.g. Inspector panel and form preferences), never for domain data.

### 8.6 Type safety

Rust `serde` structs are mirrored by TypeScript interfaces in stores
([ADR-0006](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0006-end-to-end-type-safety.md)). Changing a command's
payload requires changing both sides in one PR.

### 8.7 Performance

Large lists (EHRs, compositions, query rows) use virtualized rendering
([ADR-0023](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0023-virtualized-list-rendering-for-large-data-views.md));
response bodies in the Inspector are capped at 2 MB; terminology lookups are cached.

### 8.8 UI consistency and accessibility

Shared viewers/buttons/icons, tokens in `src/styles/tokens.css`, WCAG AA contrast
gate ([ADR-0028](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0028-color-contrast-gate.md)), no opacity-dimmed text,
keyboard navigation (`navShortcuts.ts`, PRD-0015), `aria-expanded` on disclosures.

### 8.9 Testing

| Level | Tooling |
|---|---|
| Frontend unit | Vitest (`*.test.ts` next to sources, e.g. `webtemplate`, `virtualList`, `contrast`) |
| Component / visual | Storybook stories + axe-core contrast run in CI |
| Backend unit | `cargo test` (credentials, URL building, parsing) |
| Static | `vue-tsc`, oxlint, clippy |
| Manual / integration | Local EHRBase via `docker-compose` |

### 8.10 Documentation

PRDs, ADRs, this arc42 document, README/website docs, Storybook ([ADR-0001](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0001-use-prd-and-adr-documentation.md)).
