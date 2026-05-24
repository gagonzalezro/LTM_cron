---
name: investigador-findings-2026-05-24
type: validated-research
period: 2026-05-01 to 2026-05-24
---

# Investigador Findings: May 2026 Research Update

Validated research findings from May 2026 not covered in previous entries (April 8, 2026 or earlier).

---

## [2026-05-05] Node.js 26 released - breaking changes in streams, Temporal API stable

- **Source**: [Node.js Blog](https://nodejs.org/en/blog/release/v26.0.0) / [InMotion Hosting](https://www.inmotionhosting.com/support/news/nodejs-v26-released/)
- **Confidence**: Alta
- **What changes**: Node.js 26 (released May 5, 2026) introduces breaking changes: (1) Private `_stream_*` modules removed — any package using `require('_stream_readable')`, `require('_stream_writable')`, etc. will fail. Use `require('stream').Readable` and `require('stream').Writable` instead. (2) Temporal API enabled by default and marked stable for production use — deprecates `Date` object for date/time operations. (3) V8 engine upgraded to 14.6. (4) `http.Server.prototype.writeHeader()` removed (use `writeHead()`). (5) `--experimental-transform-types` flag removed.
- **Action required**: Update dependency
- **Details**: The `_stream_*` modules breaking change is the most impactful — any dependency that requires these modules directly (check `npm ls` output) will break immediately on Node.js 26. Temporal API adoption is recommended for new code but not breaking yet (Date still works). Migrate: (1) audit dependencies for `_stream_*` usage, (2) update Node.js version in CI/CD, (3) test thoroughly in staging before prod deployment.

---

## [2026-05-01] Node.js 20 EOL deadline reached - security patches end

- **Source**: [Node.js EOL Schedule](https://nodejs.org/en/about/eol) / [HeroDevs Blog](https://www.herodevs.com/blog-posts/node-js-20-goes-eol-how-to-stay-secure-without-a-full-migration)
- **Confidence**: Alta
- **What changes**: Node.js 20.x reached end-of-life on May 1, 2026. **As of this date, the Node.js project provides no more security patches, bug fixes, or maintenance for this version.** Any vulnerability discovered in Node.js 20 after this date will not receive an official patch from the Node.js team.
- **Action required**: Urgente
- **Details**: Any production systems still running Node.js 20 are now vulnerable to unpatched exploits. Immediate migration to Node.js 22 (LTS) or 24/26 is critical. AWS Lambda nodejs20.x runtime also reaches EOL (see separate entry). For teams unable to migrate immediately, HeroDevs and NodeSource offer commercial post-EOL support through the OpenJS Foundation.

---

## [2026-05-11] TanStack npm supply chain attack - 84 malicious versions, 42 packages, GitHub Actions abuse

- **Source**: [TanStack Blog Postmortem](https://tanstack.com/blog/npm-supply-chain-compromise) / [Wiz Security](https://www.wiz.io/blog/mini-shai-hulud-strikes-again-tanstack-more-npm-packages-compromised) / [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog)
- **Confidence**: Alta
- **What changes**: On May 11, 2026 (19:20-19:26 UTC), attackers published **84 malicious npm versions across 42 @tanstack/* packages** (TanStack Router, Table, Form, Virtual, etc.). The attack vector was sophisticated: (1) Attacker created a fork (`zblgg/configuration`) and opened a pull_request_target GitHub Actions workflow trigger. (2) Workflow checked out attacker code which poisoned the GitHub Actions cache with malicious pnpm binaries. (3) Malicious binaries extracted OIDC tokens directly from runner memory (`/proc/<pid>/mem`). (4) Legitimate npm publishing pipeline used hijacked OIDC to publish malicious versions. (5) Attack detected within 20 minutes by external researcher.
- **Action required**: Urgente
- **Details**: This was the **first npm supply chain attack to carry valid SLSA provenance signatures**, making it appear legitimate. If your code depends on @tanstack/router, @tanstack/table, @tanstack/form, @tanstack/virtual, or other @tanstack/* packages, audit your lock files for versions published between 19:20-19:26 UTC on May 11, 2026. Upgrade to versions published after detection. The broader "Mini Shai-Hulud" campaign on the same date compromised 170+ npm packages total and 2 PyPI packages.

---

## [2026-05-14] node-ipc npm supply chain attack - credential stealing, 10M weekly downloads

- **Source**: [Snyk Blog](https://snyk.io/blog/malicious-node-ipc-versions-published-npm/) / [StepSecurity Blog](https://www.stepsecurity.io/blog/node-ipc-npm-supply-chain-attack) / [Datadog Security Labs](https://securitylabs.datadoghq.com/articles/node-ipc-npm-malware-analysis/)
- **Confidence**: Alta
- **What changes**: On May 14, 2026, three malicious versions of node-ipc (versions **9.1.6, 9.2.3, and 12.0.1**) were published to npm. node-ipc has ~10 million weekly downloads and is a foundational Node.js IPC library. Each malicious version contained an identical **80 KB obfuscated credential-stealing payload**. The payload: (1) Collects environment variables, host info, /etc/hosts file. (2) Exfiltrates SSH keys, Docker credentials, AWS/GCP/Azure credentials, GitHub tokens, Kubernetes config, database passwords. (3) Uses DNS-based exfiltration via `sh.azurestaticprovider.net:443` as the C2 endpoint. (4) Vector likely: compromised or re-registered maintainer email domain → npm account recovery → direct publish.
- **Action required**: Urgente
- **Details**: If your project has node-ipc as a direct or transitive dependency and you ran `npm install` or `yarn install` between the attack window (May 14, ~10-11 hours window before detection), treat all developer, CI/CD, cloud, and SSH secrets in those environments as compromised and rotate them immediately. Verify lock files for presence of 9.1.6, 9.2.3, or 12.0.1. Upgrade to the next safe version (check npm registry for patched version). This is part of the coordinated "Mini Shai-Hulud" campaign hitting 170+ packages that day.

---

## [2026-05-11 onwards] Mini Shai-Hulud supply chain campaign - 170+ npm + 2 PyPI packages compromised

- **Source**: [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/05/20/mini-shai-hulud-compromised-antv-npm-packages-enable-ci-cd-credential-theft/)
- **Confidence**: Alta
- **What changes**: The "Mini Shai-Hulud" campaign (May 11-14, 2026) was a coordinated multi-package supply chain attack spanning: TanStack (42 npm packages), node-ipc (3 versions), @antv data visualization packages (multiple versions), Mistral AI SDK packages, UiPath automation tools (65+ packages), OpenSearch packages, and Guardrails AI packages. Total: 170+ npm packages and 2 PyPI packages compromised with malicious code. The attack chain: GitHub Actions abuse (TanStack) and maintainer account compromise (node-ipc, @antv). All payloads steal credentials: env vars, cloud creds, SSH keys, API tokens, Kubernetes config.
- **Action required**: Urgente
- **Details**: Audit all production dependencies from May 11-14, 2026. Check: (1) Your full dependency tree (direct + transitive), (2) Lock files for package versions published on those dates, (3) Especially: TanStack packages, node-ipc, any @antv packages, Mistral packages, UiPath packages. Rotate all credentials in affected environments. This is the most coordinated npm/PyPI supply chain attack to date.

---

## [2026-05-08] Next.js May 2026 security release - 13 advisories (DoS, SSRF, XSS, cache poisoning)

- **Source**: [Vercel Changelog](https://vercel.com/changelog/next-js-may-2026-security-release) / [Netlify Security Advisory](https://www.netlify.com/changelog/2026-05-08-react-nextjs-security-vulnerabilities/)
- **Confidence**: Alta
- **What changes**: Vercel released emergency security patches for Next.js on May 8, 2026: versions **15.5.18 and 16.2.6** address 13 security advisories across multiple vulnerability categories: (1) Denial of Service via React Server Components. (2) Middleware and proxy bypass (App Router segment-prefetch, Pages Router i18n). (3) Server-side request forgery via WebSocket upgrades. (4) Cache poisoning. (5) Cross-site scripting via CSP nonces or `beforeInteractive` scripts. (6) Image Optimization API DoS.
- **Action required**: Actualizar dependencia
- **Details**: **Upgrade immediately** to Next.js 15.5.18 or 16.2.6. These are high-severity vulnerabilities affecting production deployments. WAF rules are not sufficient mitigation. If using Next.js 14.x or 15.x, check if your minor version is within the supported range for patches (typically latest 2-3 minor versions). Test in staging before deploying to production.

---

## [2026-05-01] CVE-2026-31431 "Copy Fail" Linux kernel vulnerability - privilege escalation in cloud environments

- **Source**: [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/05/01/cve-2026-31431-copy-fail-vulnerability-enables-linux-root-privilege-escalation) / [CISA Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)
- **Confidence**: Alta
- **What changes**: CVE-2026-31431 (CVSS HIGH) is a Linux kernel privilege escalation vulnerability affecting cloud environments and Kubernetes workloads. A working exploit is publicly available. The vulnerability allows local attackers to escalate from unprivileged user to root on systems running vulnerable kernel versions.
- **Action required**: Urgente
- **Details**: If running Linux systems in cloud (AWS, GCP, Azure, etc.) or Kubernetes clusters: update kernel immediately. Contact your cloud provider for patched kernel versions. For Kubernetes: update worker node kernel versions. The exploit is actively being used in the wild — patch priority is critical. Affects Kubernetes clusters especially, where compromised containers can escalate to host root.

---

## [2026-05-18] CVE-2026-42945 NGINX vulnerability - DoS and potential RCE via HTTP requests

- **Source**: [Help Net Security](https://www.helpnetsecurity.com/2026/05/18/ngnix-vulnerability-exploited-cve-2026-42945/) / [Security Affairs](https://securityaffairs.com)
- **Confidence**: Alta
- **What changes**: CVE-2026-42945 ("NGINX Rift") affects NGINX and allows attackers to trigger denial-of-service conditions and potentially achieve **unauthenticated remote code execution** by sending specially crafted HTTP requests to vulnerable NGINX instances. Exploitation is reliable and actively occurring in the wild.
- **Action required**: Urgente
- **Details**: Update NGINX to the latest patched version immediately. If NGINX is exposed to the internet (common in reverse proxy / load balancer roles), this is high priority. The vulnerability is actively being exploited — monitor logs for unusual HTTP request patterns. Check NGINX version: `nginx -v` and compare against advisory for patched versions.

---

## [2026-05-13] Docker Desktop CVE-2025-9074 - container escape, host file access, privilege escalation

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/docker-cve-2026-34040-lets-attackers.html) (April context applies to May patch) / [SecurityWeek](https://www.securityweek.com/docker-desktop-vulnerability-leads-to-host-compromise/)
- **Confidence**: Alta
- **What changes**: CVE-2025-9074 (CVSS 9.3, CRITICAL) in Docker Desktop (Windows, macOS) allows containers to escape and gain host access. Vulnerability: Docker Engine's internal HTTP API is accessible without authentication from containers. A malicious container can: (1) Access Docker's internal HTTP API. (2) Mount the host's entire filesystem. (3) Modify host files to escalate privileges to administrator. (4) Read arbitrary user files on the host.
- **Action required**: Urgente
- **Details**: Update Docker Desktop to **version 4.44.3 or later immediately**. If you run untrusted or user-provided containers on Docker Desktop (development machines, CI/CD), this is critical. The vulnerability allows complete host compromise. Check `docker --version` and verify you're on 4.44.3+.

---

## [2026-05-01] AWS Lambda nodejs20.x runtime EOL begins - Phase 1 of deprecation

- **Source**: [AWS Documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) / [CloudQuery Blog](https://www.cloudquery.io/blog/aws-lambda-nodejs-20-eol)
- **Confidence**: Alta
- **What changes**: AWS Lambda's nodejs20.x runtime reached end of support on May 1, 2026 (synchronized with upstream Node.js 20 EOL). Three-phase deprecation: **Phase 1 (May 1, 2026)**: AWS stops applying security patches to the nodejs20.x runtime. Functions continue to run but receive no updates. Runtime disappears from AWS Console but CLI/CloudFormation/SAM/CDK can still create/update. **Phase 2 (August 31, 2026)**: Cannot create new Lambda functions using nodejs20.x. **Phase 3 (September 30, 2026)**: Cannot update existing functions using nodejs20.x (only invoke).
- **Action required**: Urgente
- **Details**: Audit all Lambda functions with `nodejs20.x` runtime. Create a migration plan to nodejs22.x (LTS) or 24.x. Start with non-critical functions. After September 30, 2026, you'll only be able to **invoke** nodejs20.x functions, not update them, so plan migration accordingly. Use AWS Lambda PowerTuning or CloudWatch Insights to identify unused functions that can be updated risk-free first.

---

## [2026-05-01/May Patch Tuesday] Microsoft May 2026 Patch Tuesday - 118 CVEs (16 critical)

- **Source**: [Tenable Blog](https://www.tenable.com/blog/microsofts-may-2026-patch-tuesday-addresses-118-cves-cve-2026-41103) / [CrowdStrike Analysis](https://www.crowdstrike.com/en-us/blog/patch-tuesday-analysis-may-2026/)
- **Confidence**: Alta
- **What changes**: Microsoft's May 2026 Patch Tuesday (first Tuesday of May) addressed 118 CVEs: 16 CRITICAL, 102 IMPORTANT. Notable CRITICAL vulnerabilities: (1) **CVE-2026-41089** — Windows Netlogon RCE (CVSS 9.8). Netlogon is the domain authentication service; RCE allows complete domain compromise. (2) **CVE-2026-41103** — Microsoft SSO Plugin privilege escalation (CVSS 9.1) for Jira & Confluence integrations.
- **Action required**: Urgente
- **Details**: Deploy May 2026 Patch Tuesday updates immediately on all Windows systems, especially domain controllers and SSO servers (if using Microsoft SSO plugins). CVE-2026-41089 is particularly critical for Active Directory environments — prioritize domain controllers and domain-joined servers. The patch blocks Netlogon authentication bypass attacks.

---

## [2026-05-01+] CVE-2026-3298 Python asyncio buffer overflow - out-of-bounds write on Windows

- **Source**: [SentinelOne Vulnerability Database](https://www.sentinelone.com/vulnerability-database/cve-2026-3298/)
- **Confidence**: Alta
- **What changes**: CVE-2026-3298 is an out-of-bounds write vulnerability in Python's `asyncio.ProactorEventLoop` on **Windows platforms only**. The `sock_recvfrom_into()` method lacks proper boundary checking when the `nbytes` parameter is used, allowing attackers to write data beyond the allocated buffer, leading to memory corruption and potential code execution.
- **Action required**: Monitorear
- **Details**: If running Python applications using `asyncio` on Windows (especially servers accepting network input), monitor for patches from Python.org. This is primarily a Windows-specific vulnerability. Linux and macOS users are not affected. Update to the latest Python 3.14.x or 3.13.x patch release when available.

---

## [2026-05-01+] Claude Opus 4.7 released - 13% improvement in coding benchmarks

- **Source**: [Anthropic Announcement](https://www.anthropic.com/news/claude-opus-4-7) / [Releasebot Anthropic](https://releasebot.io/updates/anthropic/claude)
- **Confidence**: Alta
- **What changes**: Anthropic released **Claude Opus 4.7**, the latest flagship model in late April/early May 2026. **Pricing remains identical to Opus 4.6**: $5 per MTok input, $25 per MTok output. Performance improvement: 13% resolution lift on their 93-task coding benchmark vs. Opus 4.6. Recommended for complex reasoning and agentic coding tasks.
- **Action required**: Monitorear
- **Details**: If using Claude API for coding tasks, Opus 4.7 offers better performance at the same cost. Drop-in replacement: update API calls to use `claude-opus-4-7` instead of `claude-opus-4-6`. No changes to API format or features required.

---

## [2026-05-01+] Claude Platform on AWS GA - native Bedrock endpoint with AWS authentication

- **Source**: [AWS What's New](https://aws.amazon.com/about-aws/whats-new/2026/05/claude-platform-aws/) / [Anthropic API Release Notes](https://platform.claude.com/docs/en/release-notes/api)
- **Confidence**: Alta
- **What changes**: Anthropic launched **Claude Platform on AWS** (general availability), bringing the full Claude API feature set natively to AWS customers. Available on **Amazon Bedrock** in `us-east-1` (research preview) and through **AWS Foundry** (production GA). Endpoint: `/anthropic/v1/messages` (identical request/response format to the standalone Claude API). Authentication via AWS credentials, billing through AWS, and support for commitment retirement.
- **Action required**: Monitorear
- **Details**: For teams already on AWS: eliminates the need for separate API keys or cross-cloud integration. Benefits: AWS authentication, unified billing, zero-operator access. Supported features: Managed Agents, code execution, web tools, skills, prompt caching. Complements Bedrock integration already available via standard Anthropic API. Useful for organizations with AWS-only infrastructure policies.

---

## [2026-05-01+] Next.js 16 breaking changes - Async Request API now requires async access

- **Source**: [Next.js Upgrading Guide](https://nextjs.org/docs/app/guides/upgrading/version-16)
- **Confidence**: Alta
- **What changes**: Next.js 16 removes synchronous compatibility layer for Async Request APIs that was introduced in 15.0. In Next.js 16, all Request API access (e.g., `headers()`, `cookies()`, `draftMode()`, `searchParams()`) **must be asynchronous**. Synchronous access will fail at runtime. Additionally, Partial Prerendering (PPR) experimental flag and configuration options are removed — PPR now uses the `cacheComponents` configuration.
- **Action required**: Actualizar dependencia
- **Details**: If upgrading from Next.js 15 to 16, audit your codebase for synchronous calls to Request APIs and convert them to async. Example: `const headers = await headers()` instead of `const headers = headers()`. Update PPR configuration if applicable. Test thoroughly in staging — this is a breaking change affecting runtime behavior.

---

<!-- Summary Stats -->

## Summary

**Total new findings (May 1-24, 2026):** 13 items  
**Critical/Urgent items:** 9  
**Action required breakdown:**
- Urgente (requires immediate action): 9 items
- Actualizar dependencia (update required): 3 items  
- Monitorear (monitor/watch): 3 items

**Impact domains:**
- Supply chain attacks (npm/PyPI): 3 items
- Node.js ecosystem: 3 items
- Security CVEs: 4 items
- API/Framework releases: 3 items
