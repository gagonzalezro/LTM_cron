## [2026-05-17] Node.js 26 Released - Breaking Changes in Stream Modules and APIs

- **Source**: [nodejs.org Blog](https://nodejs.org/en/blog/release/v26.0.0) / [GitHub Release](https://github.com/nodejs/node/releases/tag/v26.0.0)
- **Confidence**: Alta
- **What changes**: Node.js 26 released May 5, 2026 removes deprecated internal stream modules (_stream_wrap, _stream_readable, _stream_writable, _stream_duplex, _stream_transform, _stream_passthrough) that have been marked deprecated for years. Also removes http.Server.prototype.writeHeader(), removes extensionless CJS exception for "type": "module" packages, and promotes DEP0201 to runtime deprecation.
- **Action required**: Monitorear / Actualizar dependencia
- **Details**: Any npm package that directly requires these internal _stream_* modules will break immediately on Node.js 26. The Temporal API is now enabled globally without --harmony flags. V8 updated to 14.6. This affects production environments running older npm packages; audit package.json for stream module dependencies before upgrading. Extensionless CJS files in type:module packages now require explicit .cjs or .mjs extension.

---

## [2026-05-17] Next.js Security Release May 6-7, 2026 - 13 Patched Vulnerabilities

- **Source**: [Vercel Changelog](https://vercel.com/changelog/next-js-may-2026-security-release) / [Netlify Security Advisory](https://www.netlify.com/changelog/2026-05-08-react-nextjs-security-vulnerabilities/)
- **Confidence**: Alta
- **What changes**: Next.js shipped coordinated security release May 6-7, 2026 patching 13 vulnerabilities: one in React Server Components, eleven in Next.js. Includes fixes for denial of service, middleware/proxy bypass, server-side request forgery (SSRF), cache poisoning, and cross-site scripting (XSS). Auth bypass flaws patched.
- **Action required**: Urgente
- **Details**: Upgrade immediately to Next.js 15.5.18 or 16.2.6. All three vulnerability classes (SSRF, cache poisoning, XSS) can lead to data breaches or session hijacking in production. Redeploy after upgrading if running on Netlify or similar platforms.

---

## [2026-05-17] Next.js 16 Breaking Changes - Async APIs and Node.js Requirements

- **Source**: [nextjs.org Upgrading Guide](https://nextjs.org/docs/app/guides/upgrading/version-16)
- **Confidence**: Alta
- **What changes**: Next.js 16 requires Node.js 18.17 or later (dropped support for 18.0-18.16). Synchronous Request APIs introduced in version 15 as compatibility layer are now fully removed in 16 — async-only access required. unstable_rootParams function removed. Dynamic data fetch caching adjusted.
- **Action required**: Actualizar dependencia
- **Details**: If running Next.js 15.x with synchronous access to Request API (headers(), cookies(), etc.), code will break on upgrade to 16. Must refactor to async patterns: `const headers = await headers()`. Update Node.js to 18.17+ before Next.js 16 upgrade. No async wrapper available in 16.

---

## [2026-05-17] Python 3.10 End-of-Life October 31, 2026 - Migration Window

- **Source**: [HeroDevs Blog](https://www.herodevs.com/blog-posts/python-3-10-end-of-life-october-2026-security-and-migration-guide) / [Dynatrace Docs](https://docs.dynatrace.com/docs/ingest-from/extensions/python-extension-runtime-update) / [Python devguide](https://devguide.python.org/versions/)
- **Confidence**: Alta
- **What changes**: Python 3.10 reaches end-of-life October 31, 2026. After that date, Python Software Foundation will not release security patches, bug fixes, or source-only updates. Cloud providers (AWS Lambda, Azure App Service, Heroku) already deprecating 3.10 runtimes. Major packages dropping 3.10 from test matrices.
- **Action required**: Monitorear
- **Details**: ~5.5 months remaining to migrate from Python 3.10. AWS Lambda staged timeline: create blocked August 2026, update blocked September 2026. Azure App Service extended support ends October 1, 2026. Migrate to Python 3.11+ now if running 3.10 in production. Libraries no longer test on 3.10, so security vulnerabilities in dependencies may not be patched for 3.10.

---

## [2026-05-17] TypeScript Overtakes JavaScript/Python on GitHub - Language Shift 2025-2026

- **Source**: [GitHub Octoverse](https://github.blog/2025-11-the-state-of-open-source/) / [DevOps.com](https://devops.com/3-notable-software-development-trends-2026-and-beyond/)
- **Confidence**: Alta
- **What changes**: TypeScript surpassed both Python and JavaScript as the most-used language on GitHub in August 2025, marking the most significant language shift in more than a decade. AI-assisted coding (Copilot agent mode) cited as key driver of TypeScript adoption over untyped languages.
- **Action required**: Monitorear
- **Details**: GitHub attributes the shift to agent-assisted coding reliability — typed languages provide better context for AI agents to generate correct code. Coupled with TypeScript 6.0 release (March 2026) and 7.0 (Go rewrite) incoming, this suggests long-term ecosystem momentum toward stricter typing.

---

## [2026-05-17] Model Context Protocol (MCP) Emerging as Standard for AI Integration

- **Source**: [Cloudflare Description](https://www.cloudflare.com/en-gb/learning/access-management/what-is-model-context-protocol/) / [DevOps.com](https://devops.com/3-notable-software-development-trends-2026-and-beyond/)
- **Confidence**: Media
- **What changes**: Model Context Protocol (MCP) emerging as open standard for connecting AI systems to external applications. Described as "USB-C port for AI applications." Multiple tools integrating MCP support (Cursor, GitHub Copilot, etc.) as of 2026.
- **Action required**: Monitorear
- **Details**: MCP allows AI agents to safely access external tools (databases, APIs, code repos) without hardcoding integrations. Early adoption by major AI IDE vendors suggests this may become the standard integration pattern. Relevant for teams building AI-assisted tooling or integrations.
