---
title: Introduction and Goals
order: 1
---

openEHR Explorer is a cross-platform desktop application (macOS, Windows, Linux)
for browsing, querying, and inspecting openEHR Clinical Data Repository (CDR)
instances such as EHRBase and Better Platform. It is positioned as a "Postman
alternative built specifically for openEHR" (see [PRD-0001](https://github.com/platzhersh/openehr-explorer/blob/main/docs/prd/PRD-0001-openehr-explorer-desktop-cdr-browser.md)).

### 1.1 Requirements Overview

| Capability | Description | PRD |
|---|---|---|
| Server profiles | Save/switch multiple CDRs, auto-detect server type and version | PRD-0001, PRD-0004, PRD-0014 |
| EHR & composition browsing | Paginated EHR list, composition grouping, search | PRD-0001, PRD-0013 |
| Composition viewer | Pretty (template-aware), JSON, and FLAT views; FLAT path panel | PRD-0001, PRD-0010 |
| Composition/EHR CRUD | Create, edit, delete via medblocks-ui forms | PRD-0003 |
| Template browser | Web Template tree, OPT XML, upload, lifecycle indicator | PRD-0008, PRD-0009, PRD-0017 |
| AQL runner | Editor with autocomplete, tabular results, saved queries, CSV export | PRD-0006 |
| Request Inspector | Raw HTTP request/response log, copy as curl | PRD-0005 |
| Terminology awareness | Resolve terminology codes | PRD-0012 |
| Settings, shortcuts, tour | Global settings, keyboard navigation, feature tour / what's new | PRD-0011, PRD-0015, PRD-0018 |
| Distribution | Signed installers, in-app updates, opt-in analytics, product website | PRD-0002, PRD-0007, PRD-0016 |

### 1.2 Quality Goals

The top quality goals, in priority order (details and scenarios in [section 10](../10-quality-requirements/)):

| # | Goal | Motivation |
|---|---|---|
| 1 | **Security & privacy** | Tool is used near clinical data; credentials must never leak, analytics must be opt-in |
| 2 | **Interoperability** | Must work across CDR vendors whose REST APIs deviate from the openEHR spec |
| 3 | **Transparency / debuggability** | The product's value is showing exactly what goes over the wire |
| 4 | **Usability** | Fast, keyboard-friendly, native-feeling desktop UX; large datasets stay responsive |
| 5 | **Maintainability** | Small, mostly volunteer-driven team; reproducible builds, strong typing, documented decisions |

### 1.3 Stakeholders

| Role | Expectation |
|---|---|
| openEHR developers / integrators (primary users) | Inspect data, run AQL, copy FLAT paths and curl commands without leaving one tool |
| Clinical-informatics / modelling staff | Browse templates and archetype structure, verify compositions |
| CDR operators | Safe, read-mostly access; no surprises on production servers |
| Contributors / maintainers | Clear structure, conventions, CI that catches regressions |
| Security / compliance reviewers | Verifiable credential handling, CSP, signed binaries |
