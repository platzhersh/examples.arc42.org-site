---
title: Risks and Technical Debt
order: 11
---

| # | Item | Impact | Mitigation / next step |
|---|---|---|---|
| R1 | **Vendor API drift** — CDRs change or deviate from the spec | Broken listings/CRUD for some versions | `server_type` branching, version detection, local EHRBase for testing; broaden CDR test matrix |
| R2 | **CDN dependency for medblocks-ui** ([ADR-0008](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0008-cdn-based-medblocks-ui.md)) | Composition forms unavailable offline; CDN compromise risk | SRI + CSP today; consider vendoring the bundle |
| R3 | **CSP allows `unsafe-inline` and broad `connect-src`** | Weaker XSS containment | Documented trade-off; CDR traffic stays in Rust; tighten when forms no longer require inline scripts |
| R4 | **AQL-based EHR listing** has limits ([ADR-0027](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0027-ehr-level-attribute-aql-limitations.md)) | Filters on some EHR attributes unavailable on some CDRs | Documented; revisit when vendors expose list endpoints |
| R5 | **Encrypted-file credential fallback** keeps its key next to the data | Weaker than keychain on systems without one | File permission hardening; keychain preferred; document in security notes |
| R6 | **Hand-mirrored Rust/TS types** | Drift between backend and frontend | Review discipline; consider generating TS types (e.g. `ts-rs`/`specta`) |
| R7 | **Duplicate ADR number 0021** (JSON viewer and Storybook) | Ambiguous references | Renumber or disambiguate in a follow-up; references in this document name the topic |
| R8 | **Small maintainer team**, many release channels | Release toil, signing-service dependency | Automated tag-driven pipelines; `RELEASING.md` |
| R9 | **Write operations against real CDRs** | Accidental data modification | Explicit confirmation dialogs ([ADR-0003](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0003-dialog-component-pattern-for-crud.md)); consider a per-profile read-only mode |
