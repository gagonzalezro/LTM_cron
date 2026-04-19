## [2026-04-13] Red Hat security advisory RHSA-2026:7896 - Node.js, minimatch, nghttp2 DoS vulnerabilities

- **Source**: [Red Hat Security Advisory RHSA-2026:7896](https://access.redhat.com/errata/RHSA-2026:7896)
- **Confidence**: Alta
- **What changes**: Red Hat issued critical DoS vulnerability patches on April 13, 2026 for nodejs:20 module affecting three libraries: (1) minimatch CVE-2026-26996 and CVE-2026-27904 - ReDoS via crafted glob patterns, (2) nghttp2 CVE-2026-27135 - DoS via malformed HTTP/2 frames after session termination, (3) Node.js CVE-2026-21710 - DoS via `__proto__` header (already covered in March 24 release).
- **Action required**: Monitor
- **Details**: Minimatch vulnerabilities are critical for any build tools using glob patterns (webpack, rollup, etc). The nghttp2 flaw affects any HTTP/2 consumers. CVE-2026-21710 was already patched in March release (v25.8.2, v24.14.1, v22.22.2, v20.20.2) but this Red Hat advisory extends coverage to RHEL systems. Systems running nodejs:20 should update to patched versions immediately.

---

## [2026-04-14] Claude Code Routines - automated cloud execution with rate limits & Desktop Redesign

- **Source**: [Anthropic Claude Blog](https://claude.com/blog/claude-code-desktop-redesign) / [9to5Mac](https://9to5mac.com/2026/04/14/anthropic-adds-repeatable-routines-feature-to-claude-code-heres-how-it-works/) / [VentureBeat](https://venturebeat.com/orchestration/we-tested-anthropics-redesigned-claude-code-desktop-app-and-routines-heres-what-enterprises-should-know/)
- **Confidence**: Alta
- **What changes**: On April 14, 2026, Anthropic released two major updates: (1) Claude Code Routines - a new feature allowing saved configurations (prompt + repos + connectors) to run automatically on Anthropic's cloud infrastructure with rate limits per subscription tier: Pro=5/day, Max=15/day, Team/Enterprise=25/day. Routines persist even if user's laptop shuts down. (2) Desktop App Redesign - UI overhaul with multi-session sidebar, drag-and-drop layout, integrated terminal/file editor, HTML/PDF preview, and faster diff viewer.
- **Action required**: Monitor
- **Details**: Routines enable CI/CD-style automation for Claude Code workflows without writing custom scripts. The rate limits are important for billing/planning - Pro users limited to 5 daily routine runs. Desktop redesign replaces need for terminal/editor switching, improving developer workflow. Migration impact is low since it's additive, but users on older Claude Code versions may face compatibility issues with routine features.

---

## [2026-04-19] No additional validated findings

<!-- Searches for React 19.3, general April 2026 trends, TypeScript migrations mostly yielded guides/articles from pre-April dates already documented in long-term-memory -->
