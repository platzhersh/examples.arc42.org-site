---
title: Quality Requirements
order: 10
---

### 10.1 Quality Tree

![Quality tree](../images/10-quality-tree.png)

### 10.2 Quality Scenarios

| ID | Scenario | Expected response |
|---|---|---|
| Q1 | A profile with Basic auth is saved | `profiles.json` contains no password; secret is in keychain or `credentials.enc` |
| Q2 | A CDR returns HTML/script in a field value | Rendered as text; no script execution; CSP blocks inline injection from external origins |
| Q3 | A request is sent with a bearer token | Inspector shows the request with `Authorization: [REDACTED]` |
| Q4 | User connects to a CDR of an unknown vendor | Works via `generic` type with configurable `api_path_prefix` |
| Q5 | A query returns tens of thousands of rows | UI remains scrollable (virtualization); Inspector body capped at 2 MB |
| Q6 | A new CDR endpoint is added | Only a Rust command + store action required; automatically visible in the Inspector |
| Q7 | A user has not consented to analytics | No events leave the machine; crash reports are not sent |
| Q8 | A release is tagged | All three platform builds, updater manifest and package-manager updates are produced without manual steps |
| Q9 | A theme colour change reduces contrast below AA | CI contrast test / axe gate fails |
| Q10 | CDR is unreachable | User sees an actionable error within the HTTP timeout; app stays usable |
