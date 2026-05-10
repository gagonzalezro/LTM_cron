# Critical Software Development Findings — April 14 to May 10, 2026

## [2026-05-05] CVE-2026-23918 - Apache HTTP/2 Double-Free (CVSS 8.8)

- **Source**: [The Hacker News](https://thehackernews.com/2026/05/critical-apache-http2-flaw-cve-2026.html) / [SecurityAffairs](https://securityaffairs.com/191759/security/apache-fixes-critical-http-2-double-free-flaw-cve-2026-23918-enabling-rce.html) / [Rescana](https://www.rescana.com/post/critical-apache-http-server-2-4-66-vulnerability-cve-2026-23918-http-2-mod-http2-double-free-enables-dos-and-remote-code-execution/) / [Apache Official](https://dailysecurityreview.com/resources/apache-cve-2026-23918-enables-dos-and-rce-in-http-2-patch-to-2-4-67/)
- **Confidence**: Alta
- **What changes**: CVE-2026-23918 (CVSS 8.8) is a double-free memory corruption bug in Apache httpd 2.4.66 mod_http2. The vulnerability triggers when a client sends HTTP/2 HEADERS frame followed by RST_STREAM with non-zero error code on same stream before stream registration. The flaw causes both nghttp2 callbacks (on_frame_recv_cb and on_stream_close_cb) to call h2_mplx_c1_client_rst → m_stream_cleanup, pushing the same h2_stream pointer twice onto spurge cleanup array. When c1_purge_streams iterates spurge and calls h2_stream_destroy → apr_pool_destroy on second entry, it hits already-freed memory.
- **Action required**: Urgente
- **Details**: DoS exploitation is trivial on any default deployment with mod_http2 and multi-threaded MPM. RCE path requires Apache Portable Runtime (APR) with mmap allocator (default on Debian-derived systems and official Apache Docker images). Exploitation for DoS confirmed in the wild as of May 5, 2026. **Mitigation**: Upgrade to Apache HTTP Server 2.4.67 or later immediately. Alternative: disable mod_http2 or switch to MPM prefork (single-threaded mode). If upgrading delayed, prioritize patching public-facing web servers.

---

## [2026-05-07] CVE-2026-44009 — vm2 Node.js Sandbox Breakout (CVSS 9.8–10.0, Unpatched)

- **Source**: [The Hacker News](https://thehackernews.com/2026/05/vm2-nodejs-library-vulnerabilities.html) / [Semgrep](https://semgrep.dev/blog/2026/calling-back-to-vm2-and-escaping-sandbox/) / [BleepingComputer](https://www.bleepingcomputer.com/news/security/critical-vm2-sandbox-bug-lets-attackers-execute-code-on-hosts/) / [Endor Labs](https://www.endorlabs.com/learn/cve-2026-22709-critical-sandbox-escape-in-vm2-enables-arbitrary-code-execution)
- **Confidence**: Alta
- **What changes**: May 2026 disclosure of **12 vm2 Node.js sandbox escape vulnerabilities** with CVSS scores up to 10.0. **CVE-2026-44009** (CVSS 9.8) — null proto exception enables sandbox escape via arbitrary code execution on host. **Critical detail**: CVE-2026-44009 and CVE-2026-44008 **remain unpatched** in all released versions including 3.11.2. Other 10 flaws patched in 3.11.2, but these two unpatched CVEs make even the latest version dangerous.
- **Action required**: Urgente
- **Details**: vm2 is widely used for sandboxing untrusted Node.js code (code execution in SaaS platforms, LLM sandboxing, serverless platforms). All versions ≤3.11.1 are exploitable for full RCE. Versions ≤3.11.2 still vulnerable to CVE-2026-44009 and CVE-2026-44008 due to lack of patches. **Remediation**: (1) For immediate term, completely disable vm2-based sandboxing; (2) Replace with kernel-level isolation: Docker containers, gVisor, or Firecracker microVMs; (3) If vm2 cannot be removed, immediately isolate it to an air-gapped environment. Do not deploy any code using vm2 with untrusted inputs until official patches released.

---

## [2026-04-28] CVE-2026-41940 — cPanel/WHM Authentication Bypass (CVSS 9.8, Zero-Day Feb 23–Apr 28)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/critical-cpanel-authentication.html) / [Help Net Security](https://www.helpnetsecurity.com/2026/04/30/cpanel-zero-day-vulnerability-cve-2026-41940-exploited/) / [Rapid7](https://www.rapid7.com/blog/post/etr-cve-2026-41940-cpanel-whm-authentication-bypass/) / [watchTowr](https://labs.watchtowr.com/the-internet-is-falling-down-falling-down-falling-down-cpanel-whm-authentication-bypass-cve-2026-41940/) / [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-41940)
- **Confidence**: Alta
- **What changes**: CVE-2026-41940 (CVSS 9.8) affects cPanel & WHM versions after 11.40 (all recent versions). Root cause: CRLF injection in login and session loading processes. Attacker manipulates `whostmgrsession` cookie by omitting expected cookie segment, avoiding encryption. Attacker injects raw `\r\n` via malicious basic authorization header; system writes session file without sanitization, allowing attacker to insert arbitrary properties (e.g., `user=root`). **Critical timeline**: Exploitation observed since **February 23, 2026** (true zero-day for ~2 months). cPanel released patch April 28, 2026.
- **Action required**: Urgente (if running cPanel/WHM)
- **Details**: Affects ~**1.5 million** internet-exposed cPanel instances per Shodan telemetry. **CISA added to KEV catalog** — federal agency deadline was May 3, 2026 (now past). Exploitation evolved from exploratory probing to mass exploitation with ransomware/malware deployment. **Remediation**: (1) Update immediately via `cPanel update` script; (2) Verify build version after patch; (3) Review access logs from Feb 23–Apr 28 for unauthorized admin access; (4) Reset all control panel passwords; (5) Check for deployed backdoors/malware. If cPanel instance was exposed during vulnerability window without monitoring, assume compromise — conduct incident response.

---

## [2026-04-28] CVE-2026-3854 — GitHub RCE via Push Options (CVSS 8.7, ~88% of GHES Vulnerable)

- **Source**: [Wiz Research](https://www.wiz.io/blog/github-rce-vulnerability-cve-2026-3854) / [The Hacker News](https://thehackernews.com/2026/04/researchers-discover-critical-github.html) / [SecurityAffairs](https://securityaffairs.com/191434/security/cve-2026-3854-github-flaw-enables-remote-code-execution.html) / [GitHub Enterprise docs](https://www.penligent.ai/hackinglabs/github-cve-2026-3854-the-rce-in-the-git-push-pipeline)
- **Confidence**: Alta
- **What changes**: CVE-2026-3854 (CVSS 8.7, CWE-77 command injection) in GitHub's internal git infrastructure. **Discovery date**: March 4, 2026 (by Wiz Research). **Public disclosure**: April 28, 2026. **Root cause**: User-supplied push option values not properly sanitized before inclusion in internal service headers. Internal header format uses delimiter character that can appear in user input → attacker injects additional metadata fields. **Attack**: Authenticated attacker can override security-critical fields: `rails_env` (sandbox bypass), `custom_hooks_dir` (hook directory redirect), `repo_pre_receive_hooks` (path traversal to arbitrary binary execution), achieving **RCE as git service user**.
- **Action required**: Actualizar dependencia (if using GitHub Enterprise Server)
- **Details**: GitHub deployed fix on GitHub.com same day (March 4). ~**88% of GitHub Enterprise Server instances vulnerable** at public disclosure. Due to multi-tenant architecture, obtaining RCE on GitHub.com exposed millions of repositories on shared storage nodes. **Impact scope**: Any organization running GHES <3.19.3. **Remediation**: Upgrade to GHES 3.19.3 or later immediately. For GitHub.com users: no action needed (fixed March 4).

---

## [2026-04-20] CVE-2026-40372 — ASP.NET Core Data Protection Elevation of Privilege (CVSS 9.1)

- **Source**: [Microsoft dotnet/announcements Issue #395](https://github.com/dotnet/announcements/issues/395) / [Microsoft dotnet/aspnetcore Issue #66410](https://github.com/dotnet/aspnetcore/issues/66410) / [The Hacker News](https://thehackernews.com/2026/04/microsoft-patches-critical-aspnet-core.html) / [GitLab Advisory Database](https://advisories.gitlab.com/nuget/microsoft.aspnetcore.dataprotection/CVE-2026-40372/)
- **Confidence**: Alta
- **What changes**: CVE-2026-40372 (CVSS 9.1, CWE-347 improper cryptographic signature verification) in Microsoft.AspNetCore.DataProtection NuGet package versions 10.0.0–10.0.6. **Root cause**: HMAC tag validation logic flaw — ciphertext with all-zero HMAC tag incorrectly accepted as valid. Attacker can forge authentication cookies and other protected payloads, causing elevation of privilege. **Attack**: Unauthenticated attacker forges authentication material (auth cookie, API key, refresh token, password reset link) as privileged user → application issues legitimately-signed tokens using new credentials.
- **Action required**: Actualizar dependencia (if using .NET 10.0.0–10.0.6)
- **Details**: Affects any .NET application loading Microsoft.AspNetCore.DataProtection 10.0.0–10.0.6 at runtime. **Remediation**: (1) Update to Microsoft.AspNetCore.DataProtection 10.0.7 immediately; (2) Redeploy affected applications; (3) **Rotate entire Data Protection key ring** (all keys must be replaced); (4) Review whether sensitive signed artifacts (auth cookies, refresh sessions, API keys, password reset links, session state) issued during vulnerable window (Microsoft.AspNetCore.DataProtection 10.0.0–10.0.6 deployments) should be invalidated or reissued; (5) Check application deployment history to identify vulnerable versions. Any organization using .NET 10.0.0–10.0.6 in production is likely affected.

---

## [2026-04-20] Docker CVE-2026-34040 — Authorization Plugin Bypass (High Severity, Incomplete Fix for CVE-2024-41110)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/docker-cve-2026-34040-lets-attackers.html) / [Cyera Research](https://www.cyera.com/blog/cyera-research-discovers-docker-authorization-bypass-that-silently-disables-security-policies) / [Hackaday](https://hackaday.com/2026/04/17/this-week-in-security-docker-auth-windows-tools-and-a-very-full-patch-tuesday/) / [Delimiter.Online](https://delimiter.online/blog/docker-cve-2026-34040/)
- **Confidence**: Alta
- **What changes**: CVE-2026-34040 is an AuthZ (authorization plugin) bypass vulnerability in Docker Engine. **Root cause**: When API request body exceeds 1 MB, Docker's middleware silently drops the body before passing request to authorization plugin. Single oversized HTTP request → every Docker authorization plugin in ecosystem silences all authorization checks → attacker gains root-level access to Docker daemon and host system. **Critical context**: This is an **incomplete fix for CVE-2024-41110** (maximum-severity from July 2024), indicating same class of flaw not fully resolved in previous patch.
- **Action required**: Actualizar dependencia (update Docker Engine immediately)
- **Details**: Affects Docker Engine versions before 29.3.1. **Remediation**: Update to Docker Engine 29.3.1 or later immediately. This is particularly critical for organizations using custom AuthZ plugins for multi-tenant Docker deployments (e.g., Kubernetes node authorization, enterprise container platforms). Any Docker daemon with AuthZ plugins enabled before 29.3.1 is vulnerable. **Mitigation**: Until patched, disable AuthZ plugins if not critical, or restrict API access to trusted networks only.

---

## [2026-04-23] Docker Desktop Privilege Escalation (CVSS 8.8)

- **Source**: [CERT-Bund Advisory](https://linuxsecurity.com/advisories/ubuntu/ubuntu-8230-1-docker) / [GovPing](https://changeflow.com/govping/data-privacy-cybersecurity/docker-desktop-flaw-allows-privilege-escalation-cvss-8-8-2026-04-24)
- **Confidence**: Alta
- **What changes**: Privilege escalation vulnerability in Docker Desktop, disclosed April 23, 2026. **CVSS 8.8**. Affects Docker Desktop versions below 4.59.0. Enables local privilege escalation on Docker Desktop installations.
- **Action required**: Actualizar dependencia
- **Details**: **Remediation**: Update Docker Desktop to 4.59.0 or later immediately. This affects developers using Docker Desktop on local machines — ensure all team members update.

---

## [2026-05-08] Next.js 16 — Breaking Changes (Turbopack Default, Async API Removals, React 19 Required)

- **Source**: [Vercel Changelog](https://vercel.com/changelog/next-js-may-2026-security-release) / [Next.js Official Docs](https://nextjs.org/docs/app/guides/upgrading/version-16) / [Netlify Changelog](https://www.netlify.com/changelog/2026-05-08-react-nextjs-security-vulnerabilities/)
- **Confidence**: Alta
- **What changes**: Next.js 16 introduces multiple **breaking changes**:
  1. **Turbopack is now the default bundler** for `next dev` and `next build` (if using `next build` with custom webpack config, build will fail unless you opt-in with `--turbopack` flag or explicitly opt-out with `--webpack`)
  2. **Synchronous access to Async Request APIs is fully removed** — functions like `headers()`, `cookies()`, `draftMode()` can no longer be called synchronously in client code during build or layout rendering
  3. **middleware.ts → proxy.ts** — rename middleware.ts to proxy.ts and rename exported function to `proxy()` (runs on Node.js runtime)
  4. **React 19 minimum required** — react and react-dom minimum version now 19
  5. **useFormState → useActionState** — useFormState deprecated in React 19 (still available but deprecated)
- **Action required**: Actualizar dependencia (if planning to upgrade Next.js)
- **Details**: Next.js 16 upgrade requires manual migration work. **Action items**: (1) Review custom webpack config — if present, test with `next build --turbopack`; (2) Refactor async API calls from synchronous to asynchronous contexts; (3) Rename middleware.ts to proxy.ts; (4) Update React/React-DOM to 19+; (5) Replace useFormState with useActionState in forms. Estimate: 2–5 hours for medium project.

---

## [2026-05-08] Next.js May 2026 Security Release — 13 Advisories (DoS, Middleware Bypass, SSRF, Cache Poisoning, XSS)

- **Source**: [Vercel Changelog](https://vercel.com/changelog/next-js-may-2026-security-release) / [Netlify Changelog](https://www.netlify.com/changelog/2026-05-08-react-nextjs-security-vulnerabilities/) / [CybersecurityNews](https://cybersecuritynews.com/next-js-react-server-vulnerabilities/)
- **Confidence**: Alta
- **What changes**: Next.js shipped **coordinated security release** addressing **13 advisories** covering: denial-of-service, middleware and proxy bypass, server-side request forgery (SSRF), cache poisoning, and cross-site scripting (XSS). Release date: May 8, 2026.
- **Action required**: Actualizar dependencia (urgent if running Next.js in production)
- **Details**: **Remediation**: Update Next.js to latest patch version immediately. Deploy fix to production ASAP due to breadth of vulnerability categories (5 different attack vectors covered by 13 advisories suggests systematic security issues). This is not a single CVE but a coordinated patch of multiple flaws.

---

## [2026-05-09] npm CVE-2026-44211 — Cline Kanban CORS WebSocket Hijacking (CVSS 3.1)

- **Source**: [GitLab Advisory Database](https://advisories.gitlab.com/npm/cline/CVE-2026-44211/)
- **Confidence**: Media
- **What changes**: CVE-2026-44211 affects Cline npm package (Kanban server), published May 9, 2026. WebSocket server starts with no Origin header validation, allowing CORS WebSocket hijacking. CVSS 3.1 — low-medium severity but affects all versions up to 2.13.0 with no fix available.
- **Action required**: Monitorear (low priority unless using Cline)
- **Details**: If project uses Cline package for Kanban functionality, monitor for patch release. **Mitigation**: If Cline server is exposed to network, implement reverse proxy with CORS origin validation.

---

## [2026-05-09] npm CVE-2026-6322 — fast-uri Host Confusion (CVSS 3.1)

- **Source**: [GitLab Advisory Database](https://advisories.gitlab.com/npm/fast-uri/CVE-2026-6322/)
- **Confidence**: Media
- **What changes**: CVE-2026-6322 affects fast-uri npm package (URI parsing library), published May 9, 2026. Versions 3.1.1 and earlier vulnerable to host confusion via percent-encoded authority delimiters. CVSS 3.1 — low severity but affects URI validation in applications using fast-uri.
- **Action required**: Monitorear (low priority unless parsing untrusted URIs)
- **Details**: If project uses fast-uri for URI validation/parsing, especially with untrusted input, update to 3.1.2+ when available. Check `npm audit` output.

---

## [2026-05-09] npm @profullstack/mcp-server — OS Command Injection (Critical, domain_lookup Module)

- **Source**: npm Security Advisories (published May 9, 2026)
- **Confidence**: Alta
- **What changes**: @profullstack/mcp-server package contains OS command injection vulnerability in `domain_lookup` module, flagged as critical on May 9, 2026.
- **Action required**: Actualizar dependencia (if using package) or remove
- **Details**: If project has @profullstack/mcp-server as dependency, remove immediately or update if patch available. Check `npm list @profullstack/mcp-server` to identify usage. This is command injection — arbitrary shell command execution risk.

---

## [2026-04-30] AWS Lambda Node.js 20 End of Life (CVSS N/A, Deprecation Event)

- **Source**: [AWS Lambda Runtime Documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) / [CloudQuery Blog](https://www.cloudquery.io/blog/aws-lambda-nodejs-20-eol) / [endoflife.date](https://endoflife.date/aws-lambda)
- **Confidence**: Alta
- **What changes**: AWS Lambda Node.js 20 reaches end-of-life on **April 30, 2026** (standard Node.js project EOL). AWS Lambda will block **function creation** using nodejs20.x runtime starting **August 31, 2026**. Existing functions continue running on nodejs20.x but no longer eligible for AWS technical support.
- **Action required**: Monitorear (plan migration by August 31, 2026)
- **Details**: **Action items**: (1) Audit all Lambda functions using nodejs20.x runtime (`aws lambda list-functions`); (2) Plan migration to nodejs22.x (current LTS) or nodejs21.x if nodejs22 not yet available in all AWS regions; (3) Test function code on new runtime (may require dependency updates); (4) Redeploy to new runtime before August 31, 2026 deadline; (5) Coordinate with CI/CD pipelines — templates may reference nodejs20.x. **Timeline**: You have ~4 months (May 10 to August 31, 2026) to migrate.

---

## Summary of Findings (April 14 – May 10, 2026)

**Critical vulnerabilities (CVSS 8+):**
- Apache HTTP/2 (CVE-2026-23918): CVSS 8.8, DoS in production confirmed
- VM2 sandbox (CVE-2026-44009): CVSS 9.8–10.0, two flaws unpatched
- cPanel auth bypass (CVE-2026-41940): CVSS 9.8, 1.5M servers affected
- GitHub RCE (CVE-2026-3854): CVSS 8.7, 88% of GHES vulnerable
- ASP.NET Core (CVE-2026-40372): CVSS 9.1, forge auth cookies
- Docker Desktop (unnamed): CVSS 8.8, local privilege escalation

**Breaking changes & deprecations:**
- Next.js 16: Turbopack default, async API removals, React 19 required
- AWS Lambda Node.js 20: EOL April 30, creation blocked August 31
- Next.js security release: 13 advisories covering 5 attack vectors

**Action priority**: (1) Apache, VM2, cPanel patches (IMMEDIATE); (2) ASP.NET, Docker, GitHub patches (THIS WEEK); (3) Next.js updates (PLAN FOR Q2 2026); (4) AWS Lambda migration (PLAN BY Q3 2026).
