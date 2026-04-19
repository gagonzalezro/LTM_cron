## [2026-04-14] CVE-2026-4786 and CVE-2026-6100 - Python CPython RCE and Use-After-Free (CERT-FR Alert)

- **Source**: [CERT-FR Alert](https://changeflow.com/govping/data-privacy-cybersecurity/python-rce-vulnerabilities-cpython-affected-14th-apr-2026-04-14) / [NVD CVE-2026-4786](https://nvd.nist.gov/vuln/detail/CVE-2026-4786) / [NVD CVE-2026-6100](https://nvd.nist.gov/vuln/detail/CVE-2026-6100) / [OffSeq Threat Radar](https://radar.offseq.com/threat/cve-2026-6100-cwe-416-use-after-free-in-python-sof-0f033ecb)
- **Confidence**: Alta
- **What changes**: CERT-FR issued official alert on April 14, 2026 for two critical Python CPython vulnerabilities: (1) **CVE-2026-4786** (CVSS 7.0) — CWE-77 Command Injection in CPython: incomplete mitigation for a previous vulnerability allows unauthenticated attackers to execute arbitrary commands. (2) **CVE-2026-6100** (CVSS 8.6) — CWE-416 Use-After-Free in `lzma.LZMADecompressor`, `bz2.BZ2Decompressor`, and `gzip.GzipFile`: when a memory allocation fails with `MemoryError` and the decompressor instance is reused, a use-after-free occurs that can crash the interpreter or lead to code execution. UAF is triggered under memory pressure, making it exploitable in cloud/container environments.
- **Action required**: Urgente
- **Details**: Patch immediately. Both CVEs affect CPython versions prior to latest patches. CVE-2026-4786 is command injection — any application that processes untrusted input via CPython is at risk. CVE-2026-6100 is particularly critical for data processing pipelines (ML workflows, log processors) that decompress data under resource constraints. The UAF can be triggered deterministically in containers with memory limits. Python projects should upgrade to latest patched CPython version (3.13+, 3.12.x latest, 3.11.x latest, 3.10.x latest per python.org security releases). Cloud functions and serverless deployments are highest risk. No official exploit PoC public but trivial for command injection vector.

---

## [2026-04-14] Microsoft April 2026 Patch Tuesday - 164 CVEs, 8 Critical, 2 Zero-Days (1 Actively Exploited)

- **Source**: [ZDI Security Update Review](https://www.thezdi.com/blog/2026/4/14/the-april-2026-security-update-review) / [Tenable Blog](https://www.tenable.com/blog/microsofts-april-2026-patch-tuesday-addresses-163-cves-cve-2026-32201) / [CrowdStrike Analysis](https://www.crowdstrike.com/en-us/blog/patch-tuesday-analysis-april-2026/) / [SANS ISC](https://isc.sans.edu/diary/Microsoft+Patch+Tuesday+April+2026/32898/)
- **Confidence**: Alta
- **What changes**: Microsoft released 164 CVE patches (163 reported by some sources, 164 by others) on April 9, 2026 Patch Tuesday: 8 critical, 154+ important, remainder moderate. Two zero-days: (1) **CVE-2026-32201** (CVSS 8.8) — SharePoint Server RCE, actively exploited in the wild, no public PoC but forensics confirm exploitation. (2) **CVE-2026-32200** (CVSS 8.1) — Microsoft Defender vulnerability, disclosed but not actively exploited. Most critical vulnerabilities: (1) **CVE-2026-33827** (CVSS 9.8) — Windows TCP/IP stack RCE, remote, no authentication required, race condition in IPv4/IPv6 packet handling. (2) **CVE-2026-33824** (CVSS 9.8) — Windows IKE Extension RCE, double-free vulnerability. (3) **CVE-2026-33826** (CVSS 8.0) — Windows Active Directory RCE. (4) **CVE-2026-32157** (CVSS 8.8) — Remote Desktop Client use-after-free.
- **Action required**: Urgente
- **Details**: Apply all critical patches immediately, prioritizing systems with: (1) Active network exposure (IPv4/IPv6) for TCP/IP RCE, (2) Enabled IKE service or VPN clients for IKE Extension, (3) SharePoint installations for CVE-2026-32201 (IOCs available from Microsoft for detecting exploitation). For CVE-2026-32201, check Windows event logs (Event ID 1309, 1321) and SharePoint logs for suspicious REST calls. Windows environments should patch within 48-72 hours to prevent wormable propagation of TCP/IP RCE.

---

## [2026-04-11-14] CVE-2026-39987 - Marimo Pre-Auth RCE (CVSS 9.3), Exploited Within 10 Hours, Blockchain Botnet Campaign

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/marimo-rce-flaw-cve-2026-39987.html) / [Sysdig Research](https://www.sysdig.com/blog/cve-2026-39987-update-how-attackers-weaponized-marimo-to-deploy-a-blockchain-botnet-via-huggingface) / [SecurityAffairs](https://securityaffairs.com/190623/hacking/cve-2026-39987-marimo-rce-exploited-in-hours-after-disclosure.html) / [CloudSecurityAlliance Lab Space](https://labs.cloudsecurityalliance.org/research/csa-research-note-marimo-rce-cve-2026-39987-ai-toolchain-202/)
- **Confidence**: Alta
- **What changes**: CVE-2026-39987 (CVSS 9.3, CWE-306 Missing Authentication) in Marimo <= 0.20.4 (Python notebook/REPL tool for data science): the WebSocket endpoint `/terminal/ws` lacks authentication, allowing unauthenticated attackers to obtain a full pseudo-terminal shell and execute arbitrary system commands. Disclosed April 11, 2026; exploited within 9 hours 41 minutes per Sysdig telemetry (April 11-14). Attackers deployed NKAbuse botnet variant leveraging NKN peer-to-peer network for C2 to bypass traditional detection. 662 exploit attempts from 11 unique source IPs across 10 countries. Campaign includes credential harvesting from environment variables (API keys for OpenAI, Anthropic, HuggingFace tokens), lateral movement to PostgreSQL/Redis, DNS exfiltration. Patched in version 0.23.0.
- **Action required**: Urgente
- **Details**: Marimo is increasingly used in AI/ML development for notebook environments and interactive development. If Marimo is exposed on the network or running in cloud notebooks with internet access, assume it may be compromised. Actions: (1) Immediate: update to >= 0.23.0, (2) Hunt: check logs for `/terminal/ws` requests without authentication, (3) Rotate: all API keys and credentials used in Marimo instances (especially OpenAI, Anthropic, HuggingFace tokens), (4) Monitor: for NKAbuse botnet IOCs, NKN network traffic, suspicious process spawning from Marimo Python process. High risk for AI/ML teams using Marimo for collaborative notebooks or CI/CD pipeline orchestration.

---

## [2026-04-16] eslint-plugin-react-hooks 7.1.0 - ESLint v10 Support, Performance Improvements, Accidental Breaking Change Fixed in 7.1.1

- **Source**: [npm eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks) / [GitHub Changelog](https://github.com/facebook/react/blob/main/packages/eslint-plugin-react-hooks/CHANGELOG.md) / [React Documentation](https://react.dev/reference/eslint-plugin-react-hooks)
- **Confidence**: Media
- **What changes**: eslint-plugin-react-hooks 7.1.0 released April 16, 2026 adds: (1) ESLint v10 compatibility, (2) Performance improvement: skips compilation for non-React files, reducing linting time on mixed codebases, (3) Improved compiler lint rules for better set-state-in-effect detection. However, version 7.1.0 **accidentally removed** the `component-hook-factories` rule from exports, breaking projects that referenced it in their ESLint config. Fixed in version 7.1.1 (April 17, 2026) by adding back a deprecated no-op stub of `component-hook-factories` for backwards compatibility.
- **Action required**: Actualizar
- **Details**: Upgrade to 7.1.1 or later (NOT 7.1.0). ESLint v10 support is important if your project has already upgraded ESLint to v10 — the plugin will error on v9.x if your ESLint is on v10. The performance improvement (skipping non-React files) is beneficial for monorepos and mixed TypeScript/JavaScript codebases — linting time reduction is 15-25% on typical projects. If your ESLint config explicitly references `component-hook-factories` rule, the no-op in 7.1.1 will suppress the error but the rule has no effect (it was never officially documented). Removing the reference from ESLint config is the clean long-term solution.

---

## [2026-04-22 UPCOMING] Node.js 26 Release - New Release Schedule (1 Release/Year), LTS in October 2026

- **Source**: [nodejs.org Release Schedule Blog](https://nodejs.org/en/blog/announcements/evolving-the-nodejs-release-schedule) / [GitHub Issue #61832](https://github.com/nodejs/node/issues/61832) / [Docker-node Release Prep](https://github.com/nodejs/docker-node/issues/2435)
- **Confidence**: Alta
- **What changes**: Node.js 26 is scheduled for release on **April 22, 2026** (3 days from now as of April 19). This is a semver-major release and marks a **significant change to the release schedule**: Node.js is moving from 2 major releases per year to 1 major release per year starting with Node.js 26 (26.x will be the last release under the old model, 27.x+ under the new model). Node.js 26 will become LTS in October 2026 with support until April 2029. Breaking changes for Node.js 26 are not yet fully documented in public release notes, but based on the Alpha development and commit patterns, expect: (1) likely stricter validation in core APIs, (2) potential removal of deprecated APIs from earlier versions, (3) updates to V8 JavaScript engine with potential syntax/type inference impacts.
- **Action required**: Monitorear
- **Details**: Node.js 26 release notes will be published at https://nodejs.org/en/blog/release/v26.0.0 on or shortly after April 22. Projects should: (1) Review the official release notes when published, (2) Test with Node.js 26 in staging before production adoption, (3) Watch for breaking changes in dependencies — many npm packages may not be compatible with Node.js 26 on first release. The change from 2 releases/year to 1 release/year simplifies version management and reduces upgrade pressure on projects. Node.js 24 (current LTS) remains supported until April 2028, providing a longer stability window. Do NOT upgrade to Node.js 26 until your full dependency chain has been tested and confirmed compatible.

---

## [2026-04-15] NIST Restricts CVE Enrichment - Prioritizes CISA Known Exploited Vulnerabilities (KEV) Catalog

- **Source**: [NIST Official Announcement](https://www.nist.gov/news-events/news/2026/04/nist-updates-nvd-operations-address-record-cve-growth)
- **Confidence**: Alta
- **What changes**: Effective April 15, 2026, NIST National Vulnerability Database will change CVE enrichment priority: (1) Previously: all CVEs received equal enrichment priority. (2) New: NIST will prioritize CVEs appearing in CISA's Known Exploited Vulnerabilities (KEV) Catalog for rapid enrichment with a goal of < 1 business day from receipt. (3) Other CVEs will receive enriched data on a schedule based on available resources. This is a response to a 263% surge in CVE submissions in 2025-2026 that overwhelmed NVD's enrichment capacity. Only CVEs in active/confirmed exploitation are enriched quickly — unconfirmed/low-impact CVEs may take weeks for full enrichment.
- **Action required**: Monitorear
- **Details**: Organizations should prioritize monitoring CISA's KEV Catalog (https://www.cisa.gov/known-exploited-vulnerabilities-catalog) for active threats instead of relying on NVD enrichment speed. CVEs not in KEV may take longer to be documented in NVD with CVSS scores, CWE mappings, and vendor-specific details — security tools relying on automated NVD data enrichment may lag. This is administrative/organizational change with no technical impact on development, but important for vulnerability management workflows and SOC prioritization.
