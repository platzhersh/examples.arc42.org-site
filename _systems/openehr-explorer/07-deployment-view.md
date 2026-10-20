---
title: Deployment View
order: 7
---

### 7.1 Infrastructure

![Deployment infrastructure](../images/07-deployment-infrastructure.png)

The app is a single self-contained installable per platform. It requires no
server infrastructure; the CDR is the user's own. `docker-compose.yml` provides
a local EHRBase for development ([`LOCAL_TESTING.md`](https://github.com/platzhersh/openehr-explorer/blob/main/LOCAL_TESTING.md)).

### 7.2 Build and release pipeline

`ci.yml` runs frontend checks (lint, format, `vue-tsc`, Vitest, build, Storybook + contrast gate)
and Rust checks (fmt, clippy `-D warnings`, tests) on every push/PR, plus a macOS build.
Pushing a `v*` tag triggers:

| Job | Output |
|---|---|
| `build-macos` / `build-windows` / `build-linux` | Installers (DMG, MSI/NSIS, AppImage/deb/rpm); Windows signed via SignPath ([ADR-0029](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0029-signpath-code-signing.md)) |
| `publish-updater-manifest` | `latest.json` for the Tauri updater |
| `publish-homebrew-cask`, `publish-scoop-bucket`, `winget.yml`, `apt-repo.yml` | Package-manager distribution |
| VirusTotal submission | Reputation scan ([ADR-0019](https://github.com/platzhersh/openehr-explorer/blob/main/docs/adr/ADR-0019-virustotal-scan-submission.md)) |
| `pages.yml` | Website + Storybook deployment |

Details: [`RELEASING.md`](https://github.com/platzhersh/openehr-explorer/blob/main/RELEASING.md).

### 7.3 Local data

| Data | Location | Sensitivity |
|---|---|---|
| Server profiles (no secrets) | `<config>/openehr-explorer/profiles.json` | Medium (URLs) |
| Secrets | OS keychain, else `credentials.enc` (AES-GCM, key file `.key`, hardened permissions) | High |
| Settings, saved queries | JSON in the config dir | Low |
| Clinical data | **Never persisted**; held in memory only while displayed | High |
