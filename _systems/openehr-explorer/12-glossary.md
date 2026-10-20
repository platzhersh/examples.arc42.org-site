---
title: Glossary
order: 12
---

| Term | Definition |
|---|---|
| **openEHR** | Open standard for health data specification, with archetype-based modelling |
| **CDR** | Clinical Data Repository — server storing openEHR data (EHRBase, Better, …) |
| **EHR** | Electronic Health Record — per-subject container identified by `ehr_id` |
| **Composition** | Unit of clinical documentation within an EHR, based on a template |
| **Archetype / Template** | Reusable clinical model / use-case-specific constraint of archetypes |
| **OPT** | Operational Template (XML) |
| **Web Template** | JSON simplification of an OPT used to build UIs and FLAT paths |
| **FLAT format** | Denormalised key/value composition representation used by SDKs |
| **AQL** | Archetype Query Language |
| **Contribution** | Atomic change set recorded by the CDR (audit trail) |
| **Server profile** | Saved connection definition: URL, `server_type`, auth, optional API path prefix |
| **Tauri** | Rust framework for desktop apps with a system webview |
| **IPC / `invoke`** | Tauri mechanism by which the webview calls Rust commands |
| **Request Inspector** | In-app log of raw HTTP traffic to the CDR |
| **medblocks-ui** | Web-component library used to render composition forms |
| **ADR / PRD** | Architecture Decision Record / Product Requirements Document |
| **arc42** | Template for architecture documentation (<https://arc42.org>) |
