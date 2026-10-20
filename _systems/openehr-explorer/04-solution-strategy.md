---
title: Solution Strategy
order: 4
---

| Goal / problem | Strategy | Where |
|---|---|---|
| Avoid CORS and keep secrets out of the webview | All CDR calls are made by Rust commands; the frontend only invokes commands | §5.2, ADR-0011 |
| Show the wire-level truth | Single `send_instrumented()` wrapper emits an event per request; Inspector store listens | §8.2, ADR-0011 |
| Cope with vendor differences | `ServerType` enum + `api_path_prefix` on each profile; URL building branches on type; AQL used for listing where REST has no list endpoint | §8.3, ADR-0010 |
| Protect credentials | OS keychain with encrypted-file fallback; secrets never in `profiles.json`; redacted in Inspector | §8.1, ADR-0015 |
| Contain XSS from hostile server data | Input validation, escaped rendering, strict CSP, SRI | §8.1, ADR-0014, ADR-0016 |
| Keep the UI maintainable | One Pinia store per domain; views compose reusable, Storybook-documented components | §5.3, ADR-0004 |
| Consistent errors | Three-layer error handling (Rust → store → view) | §8.4, ADR-0005 |
| Keep types honest across the IPC boundary | Typed IPC with manually mirrored contracts (Rust `serde` structs and TS interfaces are separate declarations; no generation step) | ADR-0006 |
| Large result sets stay responsive | Virtualized list rendering | ADR-0023 |
| Trustworthy distribution | Tag-driven CI/CD, Tauri updater, code signing, VirusTotal scan | §7, ADR-0017/0019/0029 |
