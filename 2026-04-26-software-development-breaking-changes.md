# Software Development Breaking Changes & Deprecations - April 2026

## [2026-04-26] AWS Lambda Node.js 20.x End-of-Life Timeline

- **Source**: [AWS Lambda Runtimes documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) / [endoflife.date AWS Lambda](https://endoflife.date/aws-lambda)
- **Confidence**: Alta
- **What changes**: Node.js 20 runtime in AWS Lambda reaches end-of-life on April 30, 2026. Key dates: (1) **June 1, 2026**: AWS stops allowing creation of new Lambda functions with Node.js 20.x runtime; (2) **July 1, 2026**: AWS stops allowing updates to existing Lambda functions using Node.js 20.x. After these dates, the only way to run functions is to migrate to Node.js 22.x or 24.x runtimes.
- **Action required**: Urgent
- **Details**: Audit all Lambda functions using Node.js 20.x runtime (use `aws lambda list-functions --region <region>` and filter by `Runtime: nodejs20.x`). Create migration plan for functions: test on Node.js 22.x or 24.x, update function code if needed (Node.js 22 is LTS through April 2027, safest choice), and redeploy before July 1, 2026. Automate where possible with AWS CloudFormation or IaC. AWS Lambda Python 3.9 deprecation already occurred: January 15, 2026 (no new functions) and February 15, 2026 (no updates). Amazon Linux 2 (Lambda base) reaches EOL June 30, 2026 — AWS will migrate Lambda to Amazon Linux 2023.

---

## [2026-04-26] TypeScript 6.0: Final JavaScript-Based Release with Major Deprecations (Released March 23, 2026)

- **Source**: [Microsoft TypeScript Official Blog - Announcing TypeScript 6.0](https://devblogs.microsoft.com/typescript/announcing-typescript-6-0/) / [GitHub TypeScript Issue #54500 - 6.0 Deprecation List](https://github.com/microsoft/TypeScript/issues/54500) / [Visual Studio Magazine](https://visualstudiomagazine.com/articles/2026/03/23/typescript-6-0-ships-as-final-javascript-based-release-clears-path-for-go-native-7-0.aspx)
- **Confidence**: Alta
- **What changes**: TypeScript 6.0 (released March 23, 2026) is the **final major version built on JavaScript compiler** — TypeScript 7.0 will be rewritten in Go. TypeScript 6.0 introduces mandatory deprecations that will be removed in 7.0: (1) **Compilation targets ES3 and ES5** — both deprecated; ES2015 is now the minimum target. (2) **Module systems AMD and UMD** — deprecated; ESM/CJS only. (3) **Compiler option `--baseUrl`** for module resolution — deprecated in favor of explicit path mappings. (4) **`--outFile` bundling** (ES5/AMD only) — deprecated; use bundlers instead. (5) **`--moduleResolution node`** — deprecated in favor of `node16` or `nodenext` for ESM/CJS interop. (6) **`downlevelIteration` flag** — now meaningless since ES3/ES5 are deprecated. (7) **Import assertions (`assert` keyword)** — deprecated in favor of import attributes.
- **Action required**: Monitor
- **Details**: To suppress deprecation warnings in TypeScript 6.x, add `"ignoreDeprecations": "6.0"` to `compilerOptions` in tsconfig.json. Projects targeting Node.js 18+ should use `"module": "node16"` or `"nodenext"`, and `"target": "ES2020"` or higher. Removal of old targets, module systems, and bundling options will happen in TypeScript 7.0 (estimated late 2026/early 2027). Estimate 2-4 weeks to audit and fix projects depending on deprecated settings.

---

## [2026-04-26] Django 4.2 LTS Reaches End of Support in April 2026

- **Source**: [Django Official Release Notes](https://docs.djangoproject.com/en/6.0/releases/) / [HeroDevs Django 4.2 EOL Blog](https://www.herodevs.com/blog-posts/django-4-2-is-reaching-end-of-life) / [Django Security Releases Feb 2026](https://www.djangoproject.com/weblog/2026/feb/03/security-releases/)
- **Confidence**: Alta
- **What changes**: Django 4.2 (LTS released April 2023) reaches official end-of-life in April 2026. Security patches, bug fixes, and feature releases for Django 4.2 cease. Next LTS is **Django 5.1** (not yet released as of April 2026; Django 5.2 is current stable). Django 6.0 released January 2026, Django 6.0.3 is current as of April 2026 with behavioral changes in URLField validation (no longer accepts control characters in field values).
- **Action required**: Urgent
- **Details**: Migrate Django 4.2 applications before April 2026 deadline. Recommended path: Django 4.2 → Django 5.2 (supports Python 3.10+) → Django 6.0 (requires Python 3.12+). For projects on Python 3.10 or 3.11, upgrade to Django 5.2 and plan Django 6.0 upgrade when Python 3.12+ is mandated internally. Use `python manage.py check --deploy` and run full test suites after each minor version upgrade. Breaking changes in Django 6.0: URLField no longer accepts newlines, tabs, or control characters in values — this may affect form validation rules if edge cases were previously accepted.

---

## [2026-04-26] React 19: Breaking Changes & New JSX Transform Requirement

- **Source**: [React Official Blog - React 19](https://react.dev/blog/2024/12/05/react-19) / [React 19 Upgrade Guide](https://react.dev/blog/2024/04/25/react-19-upgrade-guide) / [Medium - "I Upgraded Three Apps to React 19. Here's What Broke"](https://medium.com/@quicksilversel/i-upgraded-three-apps-to-react-19-heres-what-broke-648087c7217b)
- **Confidence**: Media
- **What changes**: React 19 introduces significant breaking changes: (1) **New JSX Transform required** — apps must use the modern JSX transform (introduced in 2020), old transform no longer supported. (2) **Error handling changes** — errors in render not re-thrown; errors caught by Error Boundary reported to console.error, uncaught errors reported to window.reportError (not thrown). (3) **TypeScript type cleanups** — removed types, reorganized types across packages. (4) **Deprecation warnings in React 19.2** — critical security patches released December 2025 for React Server Components.
- **Action required**: Monitor
- **Details**: If using class components or old JSX transform, code will break on React 19. React team recommends: first upgrade to React 18.3.1 (adds more deprecation warnings to prepare for 19), run tests, then upgrade to 19. Breaking change impact depends heavily on codebase age and dependencies (Redux, React Router, etc. also have React 19 compatibility issues). Estimate 1-3 weeks for medium-sized app.

---

## [2026-04-26] PostgreSQL 17: Deprecations & Removed Features

- **Source**: [PostgreSQL 17 Release Notes - Official Docs](https://www.postgresql.org/docs/17/release-17.html) / [PostgreSQL 17 Maintenance Changelog - Google Cloud SQL](https://www.enterprisedb.com/blog/exploring-postgresql-17-new-features-enhancements)
- **Confidence**: Alta
- **What changes**: PostgreSQL 17 (released Oct 2024, maintenance in 2026): (1) **MD5 password hashing deprecated** — CREATE ROLE and ALTER ROLE now emit warnings when setting MD5 passwords; will be removed in future major version. (2) **adminpack contrib extension removed** — was used by end-of-life pgAdmin III; if admin tools depend on adminpack functions, they will break. (3) **db_user_namespace feature removed** — simulated per-database user namespacing; rarely used, affects role/permission model if relied upon. (4) **WAL sync method fsync_writethrough on Windows removed**.
- **Action required**: Monitor
- **Details**: Audit PostgreSQL instances: check for MD5 password hashes (query `pg_authid` table for MD5 hashes), verify if adminpack is installed (`SELECT * FROM pg_extension WHERE extname = 'adminpack'`), check for db_user_namespace in postgresql.conf. If using MD5: plan migration to SCRAM-SHA-256 (now preferred). If adminpack installed: verify admin tools don't depend on it (pgAdmin 4+ doesn't). upgrade Path: PostgreSQL 16 → 17 (no data migration needed, pg_upgrade handles it).

---

## [2026-04-26] Docker Engine v29: Breaking Changes & Removal of 32-bit Raspberry Pi Support

- **Source**: [Docker Engine v29 Release Notes - Official Docs](https://docs.docker.com/engine/release-notes/29/) / [Docker Engine v28 Release Notes](https://docs.docker.com/engine/release-notes/28/)
- **Confidence**: Alta
- **What changes**: Docker Engine v29 (released ~early 2026): (1) **containerd image store is now default** for fresh installations; existing installations with traditional storage drivers (overlay2, aufs) continue to work. (2) **Go module deprecation**: github.com/docker/docker is deprecated in favor of github.com/moby/moby/client and github.com/moby/moby/api; go get github.com/docker/docker will fail in future versions. (3) **Release tagging change**: versions now tagged with `docker-` prefix (e.g., `docker-v29.0.0` instead of `v29.0.0`). (4) **Raspberry Pi OS 32-bit (armhf) support removed**: v28 is the last version supporting armhf; v29+ only supports arm64.
- **Action required**: Monitor
- **Details**: If using Docker in CI/CD or on Raspberry Pi: (1) For 32-bit Raspberry Pi, remain on Docker Engine v28 or use alternative container runtime (Podman with containerd, or k3s). (2) If customizing Docker daemon or using Go SDK: audit imports of github.com/docker/docker and start migrating to github.com/moby/moby/client. Containerd migration for fresh installs is transparent; if migrating existing docker engine instances, use `docker system prune` and be prepared for behavior changes in image storage and caching.

---

## [2026-04-26] pandas 3.0: Breaking Changes - Default String dtype & Copy-on-Write Semantics (Released Jan 21, 2026)

- **Source**: [Real Python - "pandas 3.0 Lands Breaking Changes" (Feb 2026)](https://realpython.com/python-news-february-2026/) / [pandas Official Release Notes 3.0](https://pandas.pydata.org/docs/whatsnew/v3.0.0.html)
- **Confidence**: Alta
- **What changes**: pandas 3.0 (released January 21, 2026) introduces two major breaking changes: (1) **String dtype is now default**: string columns now infer to `str` dtype instead of `object` dtype. Code relying on `.astype(str)` or object-based string operations may behave differently. (2) **Copy-on-Write (CoW) semantics mandatory**: all indexing operations now create logical copies instead of views; setting values on sliced DataFrames no longer modifies the original. Chained indexing (e.g., `df[mask][col] = value`) will now fail or behave differently than in pandas 2.x. Performance is improved but code semantics change dramatically.
- **Action required**: Urgent
- **Details**: Audit pandas code: (1) grep for chained indexing patterns (e.g., `df[df['col'] > 0]['target'] = value` — use `.loc[]` instead). (2) Test code expecting object dtype for strings — may need explicit `.astype(object)` if interacting with legacy code. (3) pandas team **strongly recommends** upgrading to pandas 2.3.x first with CoW explicitly enabled (`pd.options.mode.copy_on_write = True`) to catch breaking code early, then migrate to 3.0. Expect 1-2 weeks of testing for data pipelines. Projects with large data processing pipelines (data science, ETL) should prioritize this update.

---

## [2026-04-26] Python 3.15 (Oct 2026): UTF-8 as Default File Encoding & PEP 686

- **Source**: [Python Official Documentation - What's New in 3.15](https://docs.python.org/3/whatsnew/) / [Real Python](https://realpython.com/python-news-march-2026/)
- **Confidence**: Media
- **What changes**: Python 3.15 (scheduled October 2026 release) will implement **PEP 686**: UTF-8 will become the default encoding for `open()`, replacing locale-dependent encoding (varies by system locale, often causes encoding errors on non-UTF-8 systems). This is a long-standing source of UnicodeDecodeError bugs. Applications relying on implicit locale encoding will break.
- **Action required**: Monitor
- **Details**: Audit Python 3.14 code: (1) grep for `open()` calls without explicit `encoding=` parameter. (2) Explicitly set `encoding='utf-8'` for text files (or the required encoding if not UTF-8). (3) If legacy systems require non-UTF-8 encoding, explicitly specify it in `open(..., encoding='latin-1')` etc. This change is forward-looking — Python 3.14 (released Oct 2025) doesn't have this yet, so plan ahead. Most modern code will be unaffected (UTF-8 is near-universal), but older code or systems with non-UTF-8 locales may break.

---

## [2026-04-26] Redis 8.0: Breaking Changes - ACL Categories & Redis Search Validation

- **Source**: [Redis 8.0 Release Notes - Official Docs](https://redis.io/docs/latest/operate/rc/changelog/version-release-notes/8-0/) / [Redis Product Lifecycle](https://redis.io/docs/latest/operate/rs/installing-upgrading/product-lifecycle/)
- **Confidence**: Alta
- **What changes**: Redis 8.0 (released ~early 2026): (1) **ACL category expansion**: before Redis 8, ACL categories `@read` and `@write` did NOT include commands for Redis Search, JSON, Time Series, or Bloom filters. Starting in Redis 8, these query engine commands are included in `@read`/`@write`. Apps with ACL policies assuming these commands are excluded may have overly-permissive ACLs. (2) **Redis Search breaking changes**: enforces LIMIT validation (offset must be 0 if limit is 0), stricter parsing for ALIAS and CURSOR commands, parentheses required for exponentiation precedence in FT.APPLY expressions, invalid input now returns errors instead of empty results. Default values for reducers (AVG, COUNT, SUM, QUANTILE, etc.) may differ from Redis 7.x.
- **Action required**: Actualizar dependencia
- **Details**: If using Redis 8.0: (1) Audit ACL policies — test if Search/JSON/TimeSeries commands work as expected with existing permissions. (2) If using Redis Search: test FT.SEARCH queries, FT.ALIASADD, and FT.CURSOR usage — parenthesize exponentiation in FT.APPLY. (3) Verify reducer behavior changed (consult Redis 8 docs for new defaults). Projects using Redis primarily for caching (no Search/JSON) are largely unaffected. Redis Stack (merged into open-source in 8.0) now one distribution.

---

## [2026-04-26] Ruamel.yaml Breaking Change (January 1, 2026)

- **Source**: [Hacker News Discussion - "Ruamel.yaml gave me the first incident of 2026"](https://news.ycombinator.com/item?id=46460650)
- **Confidence**: Media
- **What changes**: Ruamel.yaml released a version with breaking changes on January 1, 2026 (New Year's Eve release). The change affected production systems of users who didn't pin versions in their `requirements.txt` / `pyproject.toml`. Incident wave occurred January 1-2, 2026.
- **Action required**: Monitor
- **Details**: If using ruamel.yaml, ensure version is pinned (e.g., `ruamel.yaml==0.18.6` not `ruamel.yaml>=0.17.0`). Review CHANGELOG on [ruamel.yaml GitHub](https://github.com/commonsware/ruamel.yaml) for the specific breaking change. Always pin YAML library versions in production — YAML parsing logic can have subtle breaking changes that cascade through config files.

