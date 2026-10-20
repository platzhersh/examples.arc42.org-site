---
title: Architecture Decisions
order: 9
---

All decisions live in [`docs/adr/`](https://github.com/platzhersh/openehr-explorer/tree/main/docs/adr/). Grouped by concern:

| Concern | ADRs |
|---|---|
| Governance | 0001 PRD/ADR docs, 0007 pinned versions |
| Frontend structure | 0002 medblocks-ui forms, 0003 dialog pattern for CRUD, 0004 store per domain, 0008 CDN medblocks-ui, 0012 CodeMirror AQL editor, 0021 JSON viewer (`ADR-0021-harmonized-json-viewer-component`), 0022 XML viewer, 0023 virtualized lists, 0025 codeblock copy |
| Backend / integration | 0005 error handling, 0006 end-to-end types, 0009 Location-header parsing, 0010 AQL-based listing, 0011 request instrumentation, 0013 OPT term-binding normalisation, 0020 contribution audit trail, 0027 EHR attribute AQL limits |
| Security & privacy | 0014 XSS hardening, 0015 credential storage, 0016 CSP + SRI, 0018 opt-in analytics |
| Delivery | 0017 Tauri updater, 0019 VirusTotal, 0029 SignPath signing |
| Quality tooling | 0021 Storybook (`ADR-0021-storybook-for-component-development`), 0028 contrast gate |
| Website | 0024 Astro + Vue site, 0026 shared Storybook/website |
