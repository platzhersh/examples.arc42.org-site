---
layout: system

permalink: /systems/openehr-explorer/

title: openEHR Explorer
tagline: Desktop browser and query tool for openEHR data repositories.

domain: Healthcare / developer tooling

highlights:
  - section: 4
    why: each goal paired with the strategy that serves it
  - section: 8
    why: ten concepts, each tied to the rule that enforces it
  - section: 10
    why: ten quality scenarios with a measurable response

main_goal: >-
  Show exactly what goes over the wire between a user and an openEHR clinical
  data repository, without leaking credentials or clinical data.

decisions:
  - All server traffic goes through the Rust core
  - One instrumented HTTP wrapper feeds a request log
  - Vendor differences isolated in a server profile

technologies:
  - Rust
  - Tauri
  - Vue

keywords:
  - adr
  - building-block
  - runtime-view
  - deployment-view
  - quality-scenario

scale: Small volunteer team · 30 ADRs · releases on all three desktop platforms

order: 120

attribution: Christian Glatthard (platzhersh)
contributed: true
licence: Apache-2.0
licence_url: https://www.apache.org/licenses/LICENSE-2.0
source_url: https://github.com/platzhersh/openehr-explorer/blob/main/docs/architecture/README.md
imported: 2026-10
---

openEHR Explorer is a Tauri desktop application for browsing, querying and
inspecting openEHR clinical data repositories such as EHRBase and Better
Platform. It is a pure client: it owns no server-side state and persists no
clinical data.

The documentation is a single Markdown file in the project's repository, split
here into the twelve arc42 sections. Its diagrams were written in Mermaid and
are published as images; the Mermaid sources are kept in this site's
repository. Links to the project's ADRs and PRDs point to GitHub, where they
stay current. The file in the project's repository remains the source of truth
and this copy is a snapshot.

Section 4 pairs every problem with its strategy and a pointer to the section
and ADR that carry it out. Section 11 lists nine risks with an impact and a
next step for each.
