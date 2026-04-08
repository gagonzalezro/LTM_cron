---
name: long-term-memory
description: Hallazgos de investigacion validados - nuevas herramientas, breaking changes, deprecaciones y actualizaciones de conocimiento descubiertas por el investigador
type: reference
---

# Long-Term Memory - Hallazgos de Investigacion

Archivo mantenido automaticamente por la skill `/investigador`. Cada entrada esta validada contra fuentes oficiales o multiples fuentes independientes.

---

## [2026-04-06] Anthropic bloquea suscripciones Claude Code en herramientas de terceros

- **Fuente**: [TechCrunch](https://techcrunch.com/2026/04/04/anthropic-says-claude-code-subscribers-will-need-to-pay-extra-for-openclaw-support/) / [Hacker News](https://news.ycombinator.com/item?id=47633396)
- **Confianza**: Alta
- **Que cambia**: Desde el 4 de abril 2026, las suscripciones Claude Pro y Max ya no funcionan dentro de herramientas agenticas de terceros (ej. OpenClaw). Solo cubren Claude Code oficial y otros productos Claude directamente.
- **Accion requerida**: Monitorear
- **Detalles**: Anthropic considero que los patrones de uso de estas herramientas no se alinean con el modelo de suscripcion. Si usas wrappers de terceros sobre Claude Code, necesitaras pagar por separado o usar la API directa.

---

## [2026-04-06] Claude Code CLI - nuevas features abril 2026

- **Fuente**: [Claude Code Changelog](https://code.claude.com/docs/en/changelog) / [GitHub Releases](https://github.com/anthropics/claude-code/releases)
- **Confianza**: Alta
- **Que cambia**: Nuevas capacidades que afectan como funciona el CLI: (1) MCP tool result persistence override via `_meta["anthropic/maxResultSizeChars"]` hasta 500K, (2) `disableSkillShellExecution` setting para deshabilitar ejecucion de shell en skills, (3) hook `PermissionDenied` que se dispara tras denegaciones del clasificador auto mode, (4) `CLAUDE_CODE_NO_FLICKER=1` env var para rendering sin flicker.
- **Accion requerida**: Monitorear
- **Detalles**: La feature de MCP result persistence es relevante para skills que manejan datos grandes (DB schemas, logs). El setting `disableSkillShellExecution` es importante para seguridad en skills compartidas.

---

## [2026-04-06] TypeScript 5.9 released - breaking changes en ArrayBuffer y type inference

- **Fuente**: [TypeScript Blog oficial](https://devblogs.microsoft.com/typescript/announcing-typescript-5-9/)
- **Confianza**: Alta
- **Que cambia**: (1) `ArrayBuffer` ya no es supertipo de `TypedArray`/`Buffer` - codigo que pasa `Buffer` donde se espera `ArrayBuffer` rompe. (2) Correccion de "type variable leaks" en inferencia puede causar nuevos errores. (3) `moduleResolution: 'node'` (clasico) ahora emite deprecation warning.
- **Accion requerida**: Actualizar dependencia
- **Detalles**: El cambio de ArrayBuffer es el mas impactante para proyectos Node.js/TypeScript. Migracion: usar `data.buffer` para extraer ArrayBuffer, o especificar tipos explicitamente como `Uint8Array<ArrayBuffer>`. Actualizar `@types/node` a la ultima version. El cambio de moduleResolution afecta proyectos que aun usen la configuracion legacy.

---

## [2026-04-06] Node.js 24 LTS - 9 CVEs corregidas en release de seguridad marzo 2026

- **Fuente**: [Node.js Security Blog](https://nodejs.org/en/blog/vulnerability/march-2026-security-releases)
- **Confianza**: Alta
- **Que cambia**: 2 HIGH, 5 MEDIUM, 2 LOW severity CVEs. Las mas criticas: (1) CVE-2026-21637 - DoS via SNICallback sin try/catch en TLS servers, (2) CVE-2026-21710 - crash por header `__proto__` en HTTP requests, (3) CVE-2026-21713 - timing side-channel en HMAC verification, (4) CVE-2026-21717 - HashDoS en V8 via JSON.parse() predecible.
- **Accion requerida**: Urgente
- **Detalles**: Versiones parcheadas: v20.20.2, v22.22.2, v24.14.1, v25.8.2. El CVE-2026-21710 (`__proto__` header) y CVE-2026-21717 (HashDoS) son especialmente peligrosos para servidores HTTP expuestos a internet. El timing attack en HMAC (CVE-2026-21713) afecta cualquier verificacion de firmas. Actualizar inmediatamente.

---

## [2026-04-06] Node.js 22 a 24 - breaking changes principales para migracion

- **Fuente**: [Node.js Migration Guide](https://nodejs.org/en/blog/migrations/v22-to-v24)
- **Confianza**: Alta
- **Que cambia**: (1) OpenSSL 3.5 con security level 2 - prohibe RSA/DSA/DH keys <2048 bits y ECC <224 bits. (2) Requiere macOS 13.5 minimo. (3) Sin binarios pre-built para Windows 32-bit ni Linux armv7. (4) APIs deprecadas: `fs.truncate` con fd, `dirent.path` -> `dirent.parentPath`, constantes `fs.F_OK` -> `fs.constants.F_OK`, opciones crypto `hash` -> `hashAlgorithm`. (5) C++ addons requieren V8 13.6 y potencialmente C++20.
- **Accion requerida**: Monitorear
- **Detalles**: El cambio de OpenSSL es el mas silencioso y peligroso - aplicaciones que usen keys RSA de 1024 bits dejaran de funcionar sin error claro. Codemods disponibles via `npx codemod run @nodejs/<nombre>`. Guia de migracion completa en GitHub issue nodejs/userland-migrations#239.

---

## [2026-04-06] React 19.2 - componente Activity y useEffectEvent hook

- **Fuente**: [React Blog](https://react.dev/blog) / [GitHub Releases](https://github.com/facebook/react/releases)
- **Confianza**: Alta
- **Que cambia**: React 19.2 (oct 2025) introduce: (1) `<Activity>` component para gestionar visibilidad y preservar estado, (2) `useEffectEvent` hook para handlers estables en effects. No hay React 20 aun - la ultima version estable es 19.2.
- **Accion requerida**: Monitorear
- **Detalles**: `<Activity>` reemplaza patrones manuales de mostrar/ocultar componentes preservando estado (similar al antiguo concepto de offscreen). `useEffectEvent` resuelve el problema clasico de refs stale en effects sin necesidad de incluir callbacks en dependency arrays. Ambos son adiciones, no breaking changes.

---

## [2026-04-06] TypeScript 6.0 GA - breaking changes masivos, strict on por defecto

- **Source**: [TypeScript Blog oficial](https://devblogs.microsoft.com/typescript/announcing-typescript-6-0/) / [GitHub Releases](https://github.com/microsoft/typescript/releases)
- **Confidence**: Alta
- **What changes**: TypeScript 6.0 (23 mar 2026) es el ultimo release con el compilador JS. Breaking changes: (1) `strict: true` por defecto en proyectos nuevos. (2) Target ES5 deprecado — minimo ahora ES2015. (3) Default `module: esnext`, `target: ES2025`. (4) `moduleResolution: classic` **eliminado**. (5) AMD, UMD y SystemJS **eliminados**. (6) `esModuleInterop`/`allowSyntheticDefaultImports` no pueden setearse a `false`. (7) `--moduleResolution node` (node10) deprecado. (8) `--baseUrl` deprecado. (9) `types` default es `[]`.
- **Action required**: Actualizar dependencia
- **Details**: Supercede la entrada de TS 5.9. Cualquier proyecto con `moduleResolution: classic`, targets ES5, o modulos AMD/UMD/SystemJS se rompe. Migrar a `moduleResolution: bundler` o `nodenext`. TS 7.0 (compilador Go, ~10x mas rapido) esta en preview en Visual Studio 2026 Insiders.

---

## [2026-04-06] React Native - arquitectura legacy completamente deprecada

- **Source**: [React Native Blog](https://reactnative.dev/blog) / [Software Mansion 2026 Predictions](https://blog.swmansion.com/react-native-in-2026-trends-our-predictions-463a837420c7)
- **Confidence**: Alta
- **What changes**: La arquitectura bridge-based (legacy) de React Native ya no puede habilitarse en versiones actuales y esta siendo eliminada del codebase.
- **Action required**: Migrar
- **Details**: Migrar a la New Architecture: Fabric renderer + JSI. Proyectos que aun dependan de modulos nativos con el bridge antiguo deben actualizar o usar wrappers de compatibilidad.

---

## [2026-04-06] Node.js 20 EOL - 30 abril 2026 (URGENTE)

- **Source**: [nodejs.org/en/about/eol](https://nodejs.org/en/about/eol) / [CloudQuery Blog](https://www.cloudquery.io/blog/aws-lambda-nodejs-20-eol) / [WordPress VIP](https://lobby.vip.wordpress.com/2026/01/29/end-of-life-for-node-js-v20/)
- **Confidence**: Alta
- **What changes**: Node.js 20 pierde todo soporte (security patches, bug fixes) el 30 abril 2026 — en 24 dias. Breaking changes al migrar a Node.js 22: (1) Import assertions (`assert`) reemplazado por import attributes (`with`). (2) Native addons requieren `npm rebuild`. (3) Streams high-water mark aumentado de 16KB a 64KB. AWS Lambda: bloquea nuevas funciones en Node.js 20 desde agosto 2026.
- **Action required**: Urgente
- **Details**: Migrar a Node.js 22 antes del 30 abril 2026. En AWS Lambda la deprecacion tiene 3 fases: bloqueo de creacion (31 ago), bloqueo de actualizaciones (30 sep 2026).

---

## [2026-04-06] Node.js - nuevo calendario de releases anual (efectivo oct 2026)

- **Source**: [nodejs.org blog](https://nodejs.org/en/blog/announcements/evolving-the-nodejs-release-schedule)
- **Confidence**: Alta
- **What changes**: A partir de octubre 2026: (1) Un major release por ano (en abril). (2) Todos los releases seran LTS — desaparece la distincion odd/even. (3) Nuevo canal Alpha (oct-mar) para breaking changes pre-release. (4) Numeros de version siguen el año calendario. Node.js 26 (abril 2026) es el ultimo bajo el modelo actual.
- **Action required**: Monitorear
- **Details**: Node.js 27 en abril 2027 sera el primero bajo el nuevo modelo. Soporte total: 36 meses por release (6 Current + 30 LTS). No cambia la duracion del LTS ni el ciclo de V8.

---

## [2026-04-06] Claude Haiku 3 (`claude-3-haiku-20240307`) retirado el 20 abril 2026 (URGENTE)

- **Source**: [Anthropic Model Deprecations](https://platform.claude.com/docs/en/about-claude/model-deprecations)
- **Confidence**: Alta
- **What changes**: Todas las llamadas a `claude-3-haiku-20240307` retornaran **error** despues del 20 abril 2026 — en 14 dias.
- **Action required**: Urgente
- **Details**: Reemplazar con `claude-haiku-4-5-20251001`. Buscar en el codigo cualquier string `claude-3-haiku-20240307` y actualizar antes del 20 abril.

---

## [2026-04-06] Anthropic - beta 1M context retirado para Sonnet 4.5/4 el 30 abril 2026

- **Source**: [Anthropic API Release Notes, 30 mar 2026](https://platform.claude.com/docs/en/release-notes/overview)
- **Confidence**: Alta
- **What changes**: El header beta `context-1m-2025-08-07` no tendra efecto en `claude-sonnet-4-5` y `claude-sonnet-4` despues del 30 abril 2026. Requests que excedan 200k tokens retornaran **error**.
- **Action required**: Urgente
- **Details**: Migrar a `claude-sonnet-4-6` o `claude-opus-4-6` que soportan 1M tokens nativamente sin header beta. Remover el header `context-1m-2025-08-07` de las llamadas.

---

## [2026-04-06] Anthropic API - nuevas features marzo 2026

- **Source**: [Anthropic API Release Notes](https://platform.claude.com/docs/en/release-notes/overview)
- **Confidence**: Alta
- **What changes**: (1) Message Batches API: `max_tokens` elevado a 300k para `claude-opus-4-6` y `claude-sonnet-4-6` con header `output-300k-2026-03-24`. (2) Models API (`GET /v1/models`) ahora retorna `max_input_tokens`, `max_tokens` y objeto `capabilities` para discovery programatico. (3) Extended thinking: nuevo campo `thinking.display: "omitted"` — omite contenido pero preserva `signature` para continuidad multi-turn.
- **Action required**: Monitorear
- **Details**: El campo `capabilities` en Models API elimina la necesidad de hardcodear limites por modelo. La opcion `display: "omitted"` en thinking acelera streaming sin romper cadenas multi-turn.

---

## [2026-04-06] Next.js 16.2 - startup ~400% mas rapido, Stable Adapter API

- **Source**: [Next.js Blog](https://nextjs.org/blog/next-16-2) / [GitHub Releases](https://github.com/vercel/next.js/releases)
- **Confidence**: Alta
- **What changes**: Next.js 16.2 (18 mar 2026): (1) ~400% faster `next dev` startup, ~50% faster rendering. (2) Turbopack: SRI support, `postcss.config.ts`, tree shaking, Server Fast Refresh. (3) Stable Adapter API para deployment cross-platform. (4) `AGENTS.md` en `create-next-app`. (5) Security patches: CVE-2026-27979 (`maxPostponedStateSize`) y CVE-2026-29057 (`http-proxy`).
- **Action required**: Actualizar dependencia
- **Details**: Actualizar para recibir los parches de seguridad CVE-2026-27979 y CVE-2026-29057. El Stable Adapter API permite colaboracion con otros proveedores de deployment sin vendor lock-in a Vercel.

---

## [2026-04-06] Axios npm supply chain attack - RAT en versiones 1.14.1 y 0.30.4

- **Source**: [SOCRadar CISO Guide](https://socradar.io/blog/axios-npm-supply-chain-attack-2026-ciso-guide/) / [MayhemCode](https://www.mayhemcode.com/2026/04/axios-npm-supply-chain-attack-2026.html)
- **Confidence**: Alta
- **What changes**: El 31 de marzo 2026, el actor UNC1069 (nexo Corea del Norte) hijackeó la cuenta npm del mantenedor principal de Axios (~100M descargas semanales) y publicó dos versiones maliciosas: `axios 1.14.1` (tag latest) y `axios 0.30.4` (tag legacy). Un script `postinstall` desplegó automáticamente los payloads **SILKBELL** (dropper) y **WAVESHAPER.V2** (backdoor RAT), cross-platform Windows/macOS/Linux.
- **Action required**: Urgente
- **Details**: Verificar inmediatamente la versión de Axios en todos los proyectos. Si se instaló `1.14.1` o `0.30.4` en el período de 2-3 horas del ataque (31 mar 2026), tratar el entorno como comprometido. Actualizar a la versión limpia más reciente. El vector inicial fue ingeniería social vía Slack/Teams falsos + instalador malicioso.

---

## [2026-04-06] Node.js 26 - fecha de release confirmada: 22 abril 2026

- **Source**: [GitHub nodejs/node issue #61832](https://github.com/nodejs/node/issues/61832)
- **Confidence**: Alta
- **What changes**: Node.js 26 se lanzará el **22 de abril de 2026** (16 días). Es el último release bajo el modelo even/odd. LTS comienza octubre 2026, EOL abril 2029. El cutoff de commits fue el 22 de marzo de 2026.
- **Action required**: Monitorear
- **Details**: Complementa la entrada anterior sobre el nuevo calendario anual. Actualizar CI/CD y dockerfiles que fijen versiones de Node.js cuando salga. Node.js 27 (abril 2027) será el primero bajo el nuevo modelo de un release anual.

---

## [2026-04-06] Python 3.14 - free-threading (sin GIL) oficialmente soportado

- **Source**: [Python 3.14 What's New](https://docs.python.org/3/whatsnew/3.14.html) / [python.org/downloads](https://www.python.org/downloads/)
- **Confidence**: Alta
- **What changes**: Python 3.14 (ultima estable: 3.14.3, feb 2026) convierte free-threading en **oficialmente soportado** (no experimental). Overhead single-thread reducido de ~40% a ~10-15%. Nuevas features: `string.templatelib` (t-strings), `concurrent.interpreters` para aislamiento via subinterpreters.
- **Action required**: Monitorear
- **Details**: El build free-threaded (`python3.14t` en Windows) sigue siendo un ejecutable separado — el GIL sigue siendo default. T-strings producen un objeto `Template` (distinto de f-strings). Python 3.10 llega a EOL el 31 oct 2026.

---

## [2026-04-06] CVE-2025-55182 "React2Shell" - RCE crítico CVSS 10.0 en React Server Components, explotación activa

- **Source**: [React Blog oficial](https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components) / [The Hacker News (2 abr 2026)](https://thehackernews.com/2026/04/hackers-exploit-cve-2025-55182-to.html)
- **Confidence**: Alta
- **What changes**: CVE-2025-55182 (CVSS 10.0) permite RCE no autenticado en aplicaciones React que usan Server Functions. A partir del 2 de abril 2026 se detectó explotación activa masiva: 766 hosts comprometidos. Los atacantes cosechan credenciales de DB, SSH keys, AWS/GCP/Azure IAM creds, API keys (Stripe, OpenAI, SendGrid, GitHub), y Kubernetes tokens.
- **Action required**: Urgente
- **Details**: Afecta `react-server-dom-webpack`, `react-server-dom-parcel`, `react-server-dom-turbopack` versiones 19.0, 19.1.0, 19.1.1, 19.2.0. Versiones parcheadas: 19.0.1, 19.1.2, 19.2.1. Para Next.js: actualizar a 14.2.35 (rama 14.x), 15.0.8+, o 16.0.11+. Apps que **no usan** React Server Components ni Server Functions no están afectadas. CVEs relacionados: CVE-2025-55184 (DoS, CVSS 7.5), CVE-2025-55183 (source code exposure, CVSS 5.3).

---

## [2026-04-06] TeamPCP - ataque a cadena de suministro de Trivy (Aqua Security), 19 marzo 2026

- **Source**: [Wiz Blog](https://www.wiz.io/blog/trivy-compromised-teampcp-supply-chain-attack) / [Microsoft Security Blog (24 mar 2026)](https://www.microsoft.com/en-us/security/blog/2026/03/24/detecting-investigating-defending-against-trivy-supply-chain-compromise/)
- **Confidence**: Alta
- **What changes**: El 19 de marzo 2026, el actor **TeamPCP** comprometió Trivy (el vulnerability scanner de Aqua Security, ~100M pulls/mes) usando credenciales retenidas de una remediación incompleta de febrero. Publicaron binarios maliciosos: Trivy **v0.69.4** y Docker images v0.69.5/v0.69.6. Force-pushed **75 de 76 tags** de `trivy-action` y todos los tags de `setup-trivy`. El payload cosechó: env vars, cloud creds (AWS/GCP/Azure), SSH keys, Kubernetes tokens, GitHub tokens, Docker Hub creds, wallet crypto. Datos exfiltrados con AES-256-CBC + RSA-4096.
- **Action required**: Urgente
- **Details**: Si tu CI/CD ejecutó Trivy v0.69.4 o usó tags de `trivy-action`/`setup-trivy` entre el 19-20 marzo, tratar como comprometido y rotar todos los secrets. IOCs: C2 domain `scan.aquasecurtiy[.]org` (typosquatting). Mitigación: pinear GitHub Actions a SHAs completos en vez de tags de versión. TeamPCP luego usó las credenciales robadas para comprometer 47+ paquetes npm adicionales.

---

## [2026-04-06] GlassWorm - 72+ extensiones maliciosas en Open VSX (marketplace VS Code)

- **Source**: [The Hacker News (mar 2026)](https://thehackernews.com/2026/03/glassworm-supply-chain-attack-abuses-72.html) / [Socket Security](https://socket.dev)
- **Confidence**: Alta
- **What changes**: Desde el 31 de enero 2026, el actor GlassWorm publicó +72 extensiones maliciosas en Open VSX (el marketplace alternativo a VS Code Marketplace, usado en VSCodium y entornos corporativos). Las extensiones suplantan linters, formatters, y AI coding assistants. Vector sofisticado: usan `extensionPack` y `extensionDependencies` para convertir extensiones inicialmente benignas en vectores de entrega transitiva (se actualizan para añadir dependencias maliciosas después de establecer confianza). Payload: roba secrets, drena crypto wallets, usa sistemas infectados como proxies. C2 vía Solana blockchain como dead drop resolver. Evita infectar sistemas en locale ruso.
- **Action required**: Monitorear
- **Details**: Open VSX ya eliminó las extensiones reportadas. Si usas Open VSX (no el marketplace oficial de Microsoft): auditar extensiones instaladas, especialmente las que imitan herramientas conocidas. Revisar actividad de red inusual hacia addresses de Solana. Las extensiones usaban Unicode invisible para ocultar código malicioso. Ejemplos de extensiones afectadas: `gvotcha.claude-code-extension`, `angular-studio.ng-angular-extension`, `turbobase.sql-turbo-tool`.

---

## [2026-04-07] GitHub Actions - campaña activa de abuso de pull_request_target + roadmap de seguridad

- **Source**: [GitHub Blog — 2026 Security Roadmap](https://github.blog/news-insights/product-news/whats-coming-to-our-github-actions-2026-security-roadmap/) / [GitHub Actions Early April 2026 Updates](https://github.blog/changelog/2026-04-02-github-actions-early-april-2026-updates/)
- **Confidence**: Alta
- **What changes**: El 2 de abril 2026, la cuenta `ezmtebo` envió 475+ PRs maliciosos en 26 horas explotando el trigger `pull_request_target`. Este trigger, a diferencia de `pull_request`, ejecuta en el contexto del repositorio base y tiene acceso a todos sus secrets incluso desde forks no confiables. GitHub respondió publicando su 2026 security roadmap con: (1) dependency locking de workflows via commit SHAs (public preview), (2) workflow execution protections via ruleset framework, (3) OIDC custom properties para cloud trust policies (GA abril 2026).
- **Action required**: Monitorear
- **Details**: Auditar inmediatamente todos los workflows con trigger `pull_request_target` que hagan checkout del código del PR — ese es el vector primario. El dependency locking y las políticas de ruleset aún están en public preview (3-6 meses para GA). Migración inmediata: reemplazar `pull_request_target` con `pull_request` donde sea posible, o asegurarse de no hacer checkout del código del PR en contexto privilegiado.

---

## [2026-04-07] Cursor 3.0 - multi-agent paralelo, Design Mode, Automations + cambio de precio Composer 2

- **Source**: [Cursor 3 Blog](https://cursor.com/blog/cursor-3) / [Cursor Changelog](https://cursor.com/changelog)
- **Confidence**: Alta
- **What changes**: Cursor 3.0 (2 abr 2026) introduce cambios arquitectónicos: (1) **Agents Window** — ejecuta múltiples agentes en paralelo en repos separados, worktrees locales, nube o SSH remoto; (2) **Design Mode** — anota elementos UI en el browser para feedback preciso al agente; (3) **Agent Tabs** — múltiples chats side-by-side; (4) **`/worktree` command** — crea git worktrees aislados para agentes; (5) **30+ plugins** de Atlassian, Datadog, GitLab, Glean, Hugging Face, monday.com, PlanetScale; (6) **Automations** — agentes always-on disparados por Slack, Linear, GitHub, PagerDuty, webhooks o schedules. Además, **Composer 2** (19 mar 2026) cambió la estructura de precios: Standard $0.50/M input + $2.50/M output; Fast (default) $1.50/M input + $7.50/M output.
- **Action required**: Monitorear
- **Details**: El tier "Fast" es ahora el default y es ~3x más caro que Standard. Equipos con usage-based billing deben auditar el impacto de costos inmediatamente. La arquitectura multi-agente paralela es un cambio fundamental — los workflows de un solo agente siguen funcionando pero las integraciones CI/CD pueden requerir revisión con el sistema de Automations.

---

## [2026-04-07] GitHub Copilot - SDK en public preview + cambio de política de privacidad (opt-in training data)

- **Source**: [GitHub Copilot SDK Public Preview](https://github.blog/changelog/2026-04-02-copilot-sdk-in-public-preview/) / [GitHub Copilot Org Custom Instructions GA](https://github.blog/changelog/2026-04-02-copilot-organization-custom-instructions-are-generally-available/) / [InfoQ — Training Data Policy](https://www.infoq.com/news/2026/04/github-copilot-training-data/)
- **Confidence**: Alta
- **What changes**: Tres cambios simultáneos el 2 de abril 2026: (1) **Copilot SDK en public preview** para Node.js/TypeScript, Python, Go, .NET y Java — permite embeber capacidades agénticas de Copilot en aplicaciones propias con tool invocation, streaming, file ops, multi-turn conversations, OpenTelemetry tracing y BYOK. (2) **Org-level custom instructions GA** — admins de Business/Enterprise pueden definir instrucciones por defecto para todos los repos. (3) **Cambio de política de privacidad efectivo 24 abril 2026**: datos de interacción (inputs, outputs, code snippets, contexto) de usuarios Free/Pro/Pro+ se usarán para entrenamiento de modelos por defecto. Business y Enterprise no afectados.
- **Action required**: Urgente
- **Details**: El punto (3) es el más crítico: cualquier desarrollador en plan individual que trabaje con código propietario o sensible debe ir a Settings > Privacy y deshabilitar el uso para entrenamiento ANTES del 24 de abril. El SDK (punto 1) consume quotas de premium requests. La org custom instructions (punto 2) es aditiva.

---

## [2026-04-07] Google Gemini API - modelos Pro detrás de paywall + deprecaciones múltiples

- **Source**: [Gemini API Changelog](https://ai.google.dev/gemini-api/docs/changelog) / [Gemini API Deprecations](https://ai.google.dev/gemini-api/docs/deprecations)
- **Confidence**: Alta
- **What changes**: Dos cambios con impacto breaking: (1) **Pro models paywalled** (efectivo 1 abril 2026): el free tier ahora solo cubre modelos Flash y Flash-Lite — cualquier código usando `gemini-3-pro`, `gemini-2.5-pro` u otros modelos Pro con API key gratuita falla con error. Se introdujeron nuevos tiers Flex y Priority inference y planes Prepay/Postpay. (2) **Deprecaciones ya activas**: `gemini-2.5-flash-lite-preview-09-2025` shutdowneado el 31 mar 2026 (migrar a `gemini-3.1-flash-lite-preview`), `gemini-3-pro-preview` shutdowneado el 9 mar 2026 (migrar a `gemini-3.1-pro-preview`). **Próximas deprecaciones**: `gemini-2.0-flash` y variantes sunset 1 jun 2026; `gemini-2.5-pro` (stable) sunset 17 jun 2026.
- **Action required**: Actualizar dependencia
- **Details**: Cualquier integración con Gemini API debe auditarse: (a) verificar si usa modelos ya deprecados (errores inmediatos), (b) verificar si usa modelos Pro en free tier (errores desde 1 abril), (c) planificar migración de `gemini-2.0-flash` y `gemini-2.5-pro` antes de junio 2026.

---

## [2026-04-07] Claude Code 2.1.91-2.1.92 - nuevas features de hook system y Bedrock setup

- **Source**: [Claude Code CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- **Confidence**: Alta
- **What changes**: Versiones 2.1.91-2.1.92 (1-4 abr 2026) añaden sobre las features anteriores: (1) Campo `"defer"` en decisiones de permisos para hooks `PreToolUse` — el hook puede diferir la decisión en vez de aprobar/denegar. (2) Campo condicional `if` para hooks (desde v2.1.85) — ejecuta hooks condicionalmente sin modificar la lógica del agente. (3) Write tool 60% más rápido en diff computation para archivos grandes con tabs/`&`/`$`. (4) `forceRemoteSettingsRefresh` — nueva policy setting para forzar refresco de settings remotos. (5) Asistente interactivo de setup para **AWS Bedrock** (wizard). (6) `/cost` ahora muestra breakdown por modelo y por cache hit.
- **Action required**: Monitorear
- **Details**: El campo `"defer"` en hooks es especialmente útil para pipelines de aprobación en CI/CD que necesitan consultar un sistema externo antes de decidir. El wizard de Bedrock reduce significativamente la fricción de setup en entornos AWS. Actualizar a 2.1.92 para recibir todas las mejoras.

---

## [2026-04-07] Mistral Small 4 - modelo unificado 256k contexto + Voxtral TTS API

- **Source**: [Mistral API Changelog](https://docs.mistral.ai/getting-started/changelog)
- **Confidence**: Alta
- **What changes**: Dos releases en marzo 2026: (1) `mistral-small-2603` (16 mar) — modelo híbrido MoE (119B total, 6B activos/token) que consolida instruction-following, razonamiento, visión y coding en un solo endpoint. 256k de contexto. Afirma 40% reducción en latencia vs modelos previos. Reemplaza cuatro modelos separados. (2) `voxtral-tts-2603` (23 mar) — TTS API con zero-shot voice cloning, 9 idiomas, streaming de baja latencia y voces custom. Precio: $0.016/1k caracteres.
- **Action required**: Monitorear
- **Details**: El `mistral-small-2603` es relevante para quienes mantienen routing lógico separado por capacidad (razonamiento vs visión vs coding) con Mistral — pueden simplificar a un único endpoint. El TTS `voxtral-tts-2603` abre un nuevo vector de integración para aplicaciones de voz. Zero-shot voice cloning es notable para personalización.

---

## [2026-04-07] Anthropic API - features adicionales de abril 2026 no capturadas previamente

- **Source**: [Anthropic API Release Notes](https://docs.anthropic.com/en/release-notes/overview) / [Releasebot Anthropic April 2026](https://releasebot.io/updates/anthropic)
- **Confidence**: Alta
- **What changes**: Cuatro cambios adicionales a la API de Anthropic confirmados en los release notes de abril 2026, no incluidos en la entrada anterior de marzo: (1) Web search tool y web fetch ahora **GA sin beta headers** — se elimina la necesidad de pasar cualquier header beta para activarlos. (2) Web search/fetch soportan **dynamic filtering** via code execution integrado — filtra resultados antes de que lleguen al contexto, mejora rendimiento y reduce costo de tokens. (3) **API code execution es gratuito** cuando se usa junto con web search o web fetch (solo se cobra cuando se usa de forma standalone). (4) **Fine-grained tool streaming** ahora GA en todos los modelos y plataformas (no solo Opus/Sonnet).
- **Action required**: Monitorear
- **Details**: El punto (1) simplifica integraciones existentes: remover headers beta de web search. El punto (3) hace económicamente viable usar code execution para pipelines que ya usen web search/fetch. El dynamic filtering (2) es especialmente útil para reducir context bloat en búsquedas con muchos resultados. Los cambios complementan y actualizan la entrada anterior del 2026-04-06 sobre features de marzo.

---

## [2026-04-07] Claude Code - filtración de código fuente vía error de packaging en npm (31 mar 2026)

- **Source**: [CNBC](https://www.cnbc.com/2026/03/31/anthropic-leak-claude-code-internal-source.html) / [The Hacker News](https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html) / [The Register](https://www.theregister.com/2026/04/06/anthropic_code_leak_kettle_podcast/)
- **Confidence**: Alta
- **What changes**: El 31 de marzo de 2026, Anthropic expuso accidentalmente 512.000 líneas de código TypeScript (1.906 archivos) de Claude Code a través de un archivo `.map` de 59.8 MB incluido en el paquete npm `@anthropic-ai/claude-code` versión 2.1.88. El error ocurrió porque Bun genera source maps completos por defecto y el `.npmignore` no los excluía. Anthropic confirmó que no se expusieron credenciales ni datos de clientes. El código fue rápidamente espejado en GitHub y analizado por investigadores y actores maliciosos.
- **Action required**: Monitorear
- **Details**: El código filtrado reveló múltiples feature flags de capacidades inéditas: revisión de sesión automática, "persistent assistant" en background mode, y control remoto de Claude desde móvil/browser. Para equipos que usan Claude Code: no hay acción de seguridad inmediata requerida (no hay credenciales expuestas), pero el análisis del código puede revelar comportamientos internos del agente que no estaban documentados. Actualizar a versiones ≥2.1.89 que excluyen los source maps.

---

## [2026-04-07] LiteLLM supply chain attack por TeamPCP - PyPI comprometido 24 mar 2026

- **Source**: [LiteLLM Security Update oficial](https://docs.litellm.ai/blog/security-update-march-2026) / [Snyk Blog](https://snyk.io/blog/poisoned-security-scanner-backdooring-litellm/) / [Kaspersky Blog](https://www.kaspersky.com/blog/critical-supply-chain-attack-trivy-litellm-checkmarx-teampcp/55510/) / [Cycode](https://cycode.com/blog/lite-llm-supply-chain-attack/)
- **Confidence**: Alta
- **What changes**: Extiende la entrada existente sobre TeamPCP/Trivy. El 24 de marzo de 2026, TeamPCP usó credenciales PyPI robadas mediante el CI/CD comprometido de LiteLLM (que usaba Trivy) para publicar versiones maliciosas: `litellm==1.82.7` y `litellm==1.82.8`. Estuvieron activas ~40 minutos (10:39–11:19 UTC). El payload era un archivo `.pth` (`litellm_init.pth`) que se ejecuta automáticamente en cada proceso Python al arrancar, robando todas las env vars, SSH keys, credenciales cloud y otros secrets. LiteLLM tiene ~95M de descargas mensuales y es una dependencia crítica de muchos stacks AI.
- **Action required**: Urgente
- **Details**: Si instalaste `litellm==1.82.7` o `litellm==1.82.8`, tratar el entorno como comprometido y rotar todos los secrets. La versión segura es `v1.83.0`+, construida con un nuevo pipeline CI/CD aislado. Los deployments via Docker image oficial de LiteLLM Proxy **no fueron afectados** (usan requirements.txt pinneados). Buscar `litellm_init.pth` en entornos Python como indicador de compromiso. Esta cadena es directa consecuencia del ataque previo a Trivy.

---

## [2026-04-07] CVE-2026-23745 - node-tar ≤7.5.2: arbitrary file overwrite y symlink poisoning (CVSS 8.2)

- **Source**: [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-23745) / [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/tar/CVE-2026-23745/) / [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2026-23745/)
- **Confidence**: Alta
- **What changes**: CVE-2026-23745 (CVSS 8.2, HIGH) afecta `node-tar` ≤7.5.2. La librería no sanitiza el campo `linkpath` de entradas hardlink y SymbolicLink cuando `preservePaths: false` (el modo seguro por defecto). Esto permite que archivos TAR maliciosos escriban fuera del directorio de extracción (path traversal a rutas absolutas como `/etc/passwd`), llevando a **arbitrary file overwrite** y potencial **RCE vía manipulación de archivos de configuración**.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `tar` (node-tar) a `>=7.5.3`. El paquete `tar` es una dependencia transitiva de npm CLI, Node-RED, y muchos otros proyectos — ejecutar `npm audit` para verificar. GHSA: `GHSA-8qq5-rm4j-mr97`. Descubierto en enero 2026, parcheado en 7.5.3. La vulnerabilidad se activa únicamente al extraer archivos TAR maliciosos (no afecta archivos TAR de confianza).

---

## [2026-04-07] CVE-2026-5281 - Chrome/Chromium use-after-free en Dawn/WebGPU, explotación activa, CISA deadline 15 abr

- **Source**: [Help Net Security](https://www.helpnetsecurity.com/2026/04/01/google-chrome-zero-day-cve-2026-5281/) / [The Hacker News](https://thehackernews.com/2026/04/new-chrome-zero-day-cve-2026-5281-under.html) / [SOCRadar](https://socradar.io/blog/cve-2026-5281-chrome-webgpu-zero-day/) / [Security Affairs](https://securityaffairs.com/190265/hacking/google-fixes-fourth-actively-exploited-chrome-zero-day-of-2026.html)
- **Confidence**: Alta
- **What changes**: CVE-2026-5281 es un use-after-free (CWE-416) en Dawn, la implementación WebGPU de Chromium. Permite a un atacante que comprometió el renderer process ejecutar código arbitrario via página HTML maliciosa. Explotación activa confirmada desde el 10 de marzo de 2026. Es el **cuarto zero-day de Chrome explotado en 2026**. CISA lo añadió al KEV catalog el 1 de abril de 2026.
- **Action required**: Urgente
- **Details**: Actualizar Chrome a `v146.0.7680.177` (Linux) o `v146.0.7680.178` (Windows/macOS). **Todos los navegadores basados en Chromium están afectados**: Microsoft Edge, Brave, Opera, Vivaldi — actualizar también. CISA deadline para agencias federales: **15 de abril de 2026** (8 días). El parche fue publicado out-of-band el 1 de abril de 2026.

---

## [2026-04-07] CVE-2025-15379 - MLflow 3.8.0: RCE crítico CVSS 10 por command injection en model serving

- **Source**: [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2025-15379/) / [TheHackerWire](https://www.thehackerwire.com/mlflow-critical-rce-via-model-artifact-command-injection/)
- **Confidence**: Alta
- **What changes**: CVE-2025-15379 (CVSS 10.0, CRÍTICO) afecta MLflow 3.8.0. La función `_install_model_dependencies_to_env()` interpola directamente especificaciones de dependencias del archivo `python_env.yaml` de un artifact en un comando shell sin sanitización. Un atacante que controle el artifact (modelo) puede ejecutar código arbitrario en el servidor de inferencia. Publicado el 30 de marzo de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar MLflow a `>=3.8.2`. El fix introduce escaping con `shlex` antes de construir el comando shell. El vector de ataque requiere que el atacante pueda subir o proporcionar un model artifact malicioso — especialmente crítico en plataformas MLOps donde usuarios externos pueden subir modelos. El parámetro `env_manager=LOCAL` es necesario para que la vulnerabilidad sea explotable.

---

## [2026-04-07] CVE-2026-33195 - Ruby on Rails Active Storage: path traversal en DiskService (CVSS 8.0)

- **Source**: [Rails Security Announcement oficial](https://discuss.rubyonrails.org/t/cve-2026-33195-possible-path-traversal-in-active-storage-diskservice/90904) / [GitLab Advisory](https://advisories.gitlab.com/pkg/gem/activestorage/CVE-2026-33195/) / [CVEReports](https://cvereports.com/reports/CVE-2026-33195)
- **Confidence**: Alta
- **What changes**: CVE-2026-33195 (CVSS 8.0, HIGH) afecta `activestorage` en Rails 7.2.x, 8.0.x y 8.1.x. `DiskService#path_for` no valida que el path resuelto permanezca dentro del directorio raíz de storage. Si un blob key contiene secuencias de path traversal (`../`), permite leer, escribir o borrar archivos arbitrarios del servidor — incluyendo RCE por sobreescritura de archivos de configuración.
- **Action required**: Actualizar dependencia
- **Details**: Versiones parcheadas: Rails `7.2.3.1`, `8.0.4.1`, `8.1.2.1`. La vulnerabilidad requiere que la aplicación use blob keys controlados por el usuario — si los keys son generados internamente por Rails, el riesgo es bajo. Aplicaciones que permitan a usuarios especificar paths de storage están en máximo riesgo. Verificar si se usa `DiskService` (no aplica a S3/GCS/Azure adapters).

---

## [2026-04-07] Anthropic API - data residency controls GA: parámetro inference_geo

- **Source**: [Anthropic API Release Notes](https://platform.claude.com/docs/en/release-notes/overview) / [Releasebot Anthropic](https://releasebot.io/updates/anthropic)
- **Confidence**: Alta
- **What changes**: Anthropic introdujo controles de **data residency** con el nuevo parámetro `inference_geo` en la API. Permite especificar dónde se ejecuta la inferencia del modelo. La opción `US-only` está disponible a 1.1x el precio estándar, aplicable a modelos publicados después del 1 de febrero de 2026.
- **Action required**: Monitorear
- **Details**: Relevante para organizaciones con requisitos de compliance de datos (GDPR, HIPAA, contratos que exigen procesamiento en US). Añadir `inference_geo: "us"` en los parámetros de la request para forzar inferencia en US. El surcharge del 10% aplica por request. Complementa las features de abril 2026 documentadas en la entrada anterior.

---

## [2026-04-08] OpenAI - Assistants API sunset agosto 2026 + múltiples deprecaciones

- **Source**: [OpenAI Developer Community - Assistants API deprecation](https://community.openai.com/t/assistants-api-beta-deprecation-august-26-2026-sunset/1354666) / [OpenAI Deprecations oficial](https://developers.openai.com/api/docs/deprecations)
- **Confidence**: Alta
- **What changes**: OpenAI anuncia varias deprecaciones críticas con fechas concretas: (1) **Assistants API** — deprecated, sunset **26 agosto 2026**. Migrar a la nueva **Responses API** (soporta multi-step workflows, tool integrations, agentes). (2) **Realtime API Beta** — sunset **7 mayo 2026** (~4 semanas). (3) **DALL-E model snapshots** — sunset **12 mayo 2026** (~5 semanas). (4) **Videos API & Sora 2** — deprecated 24 mar 2026, sunset **24 septiembre 2026**. (5) **GPT-5.2 Thinking** — sunset **5 junio 2026**.
- **Action required**: Migrate
- **Details**: Las más urgentes son Realtime API (7 mayo) y DALL-E snapshots (12 mayo) — aproximadamente un mes para migrar. Assistants API tiene mayor margen (agosto) pero es la más impactante por el volumen de integraciones. La Responses API incluye shell tool, agent execution loop, hosted container workspace, context compaction y reusable agent skills. Auditar todas las integraciones OpenAI para identificar las afectadas.

---

## [2026-04-08] React Router v7 - soporte Vite 8 + breaking change en RSC Framework Mode

- **Source**: [React Router CHANGELOG.md oficial](https://reactrouter.com/changelog) / [Releasebot React Router April 2026](https://releasebot.io/updates/remix/react-router)
- **Confidence**: Alta
- **What changes**: Release del 2 de abril 2026: (1) **Soporte Vite 8** — flag `future.unstable_viteEnvironmentApi` estabilizado como `future.v8_viteEnvironmentApi` en `react-router.config.ts`. (2) **Breaking change en RSC Framework Mode (unstable)**: proyectos que ya adoptaron el RSC Framework Mode en estado unstable deben actualizar sus route modules para exportar las nuevas anotaciones requeridas. (3) `fetcher.unstable_reset()` estabilizado como `fetcher.reset()`.
- **Action required**: Monitorear
- **Details**: El breaking change solo afecta a equipos que adoptaron el RSC Framework Mode *unstable* anticipadamente. Para el resto, la actualización es no-breaking. Migración: cambiar `future.unstable_viteEnvironmentApi` → `future.v8_viteEnvironmentApi` en config; actualizar exports de route modules si se usa RSC mode. El soporte de Vite 8 es necesario para usar las últimas features de Vite.

---

## [2026-04-08] Vercel - Observability Plus sin fee base + nuevos modelos en AI Gateway

- **Source**: [Vercel Changelog](https://vercel.com/changelog) / [Vercel Weekly 2026-04-06](https://community.vercel.com/t/vercel-weekly-2026-04-06/37604)
- **Confidence**: Alta
- **What changes**: Dos cambios relevantes en la semana del 6 de abril 2026: (1) **Observability Plus pricing**: eliminado el fee fijo de $10/mes — ahora se paga solo por eventos de observabilidad recolectados (modelo pay-per-use). (2) **Nuevos modelos en Vercel AI Gateway**: `Qwen 3.6 Plus` de Alibaba (1M contexto, capacidades agénticas, mejoras en tool-calling y multilingual) y `Gemma 4 26B` (MoE) + `Gemma 4 31B` (Dense) de Google.
- **Action required**: Monitorear
- **Details**: El cambio de Observability Plus puede reducir costos para proyectos con bajo volumen de eventos, pero aumentarlos para proyectos con alto volumen que antes pagaban flat rate. Revisar el uso actual antes de asumir ahorro. Los nuevos modelos en AI Gateway amplían las opciones para workflows AI sin necesidad de manejar múltiples integraciones de proveedores.

---

## [2026-04-08] GitHub Copilot cloud agent - commits firmados + agent firewall (abril 2026)

- **Source**: [GitHub Changelog - April 2026](https://github.blog/changelog/month/04-2026/) / [GitHub Copilot What's New](https://github.com/features/copilot/whats-new)
- **Confidence**: Alta
- **What changes**: Complementa la entrada del 2026-04-07 sobre Copilot SDK. Nuevas capacidades del Copilot cloud agent en abril 2026: (1) **Commits firmados**: todos los commits del cloud agent aparecen como **Verified** en GitHub (firmados con clave de Anthropic/GitHub). (2) **Agent firewall integrado**: bloquea acceso a internet del agente para proteger contra prompt injection y data exfiltration. (3) **@copilot en PRs**: mencionar `@copilot` en comentarios de PR instruye al agente a hacer cambios en su propio entorno cloud. (4) **Copilot coding agent management REST API** (public preview): programmatic control de acceso por repo para org owners.
- **Action required**: Monitorear
- **Details**: El agent firewall es especialmente relevante para entornos enterprise donde el código puede tener datos sensibles. Evaluar si la restricción de internet del agente impacta workflows que necesiten que Copilot busque documentación externa. Los commits verificados mejoran auditoría en repos con requisitos de compliance.

---

## [2026-04-08] Python 3.14.4 - patch de seguridad: 3 CVEs + HTTP header injection

- **Source**: [Python Release Python 3.14.4 | Python.org](https://www.python.org/downloads/release/python-3144/)
- **Confidence**: Alta
- **What changes**: Python 3.14.4 (7 abril 2026) incluye ~337 bugfixes y las siguientes correcciones de seguridad: (1) **CVE-2026-4224** — crash por recursión C no acotada en `xml.parsers.expat` con `ElementDeclHandler()`. (2) **CVE-2026-3644** — caracteres de control no rechazados en `http.cookies.Morsel.update()` y `js_output()`. (3) **CVE-2026-2297** — `SourcelessFileLoader` ahora usa `io.open_code()` al abrir `.pyc`. (4) HTTP header injection en `wsgiref.handlers` (sin CVE asignado). (5) Memory DoS potencial en `http.server` CGI handler en Windows. (6) Guiones iniciales rechazados en URLs de `webbrowser.open()`.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a Python 3.14.4 si se usa alguno de: `xml.parsers.expat`, `http.cookies`, `http.server` CGI handler (especialmente en Windows), o `wsgiref`. El CVE-2026-4224 (crash en expat) es el más crítico para servicios que parsean XML desde fuentes no confiables. Complementa la entrada anterior de Python 3.14 (free-threading, t-strings).

---

## [2026-04-08] Vercel - actualizaciones adicionales 7-8 abril 2026

- **Source**: [Vercel Changelog](https://vercel.com/changelog) / [Vercel Weekly 2026-04-06](https://community.vercel.com/t/vercel-weekly-2026-04-06/37604)
- **Confidence**: Alta
- **What changes**: Items adicionales a la entrada anterior del Vercel Weekly, todos confirmados en el changelog oficial: (1) **Team-wide Zero Data Retention en AI Gateway** — las org pueden forzar ZDR y controles de prompt training para todos los providers soportados desde el nivel de equipo (relevante para compliance). (2) **Vercel Sandbox**: hasta **32 vCPU + 64 GB RAM** para Enterprise (vs. límites anteriores). (3) **Turborepo 2.9**: mejora de rendimiento de hasta **96%** via AI agents + Vercel Sandboxes; nueva integración que pareliza tareas con agentes. (4) **Go support**: deploy de backends Go en Vercel sin configuración adicional (zero-config). (5) **CDN**: ahora respeta headers `Cache-Control` de orígenes externos por defecto (cambio de comportamiento — revisar si dependías del behavior anterior). (6) **Queues**: TTL de mensajes extendido a 7 días.
- **Action required**: Monitorear
- **Details**: El cambio de CDN (punto 5) es el más propenso a causar comportamiento inesperado — si tienes orígenes externos con headers de cache permisivos, Vercel ahora los respetará automáticamente en lugar de usar sus propios defaults. Revisar la configuración de `Cache-Control` en orígenes externos antes de hacer deploy en Vercel.

---

## [2026-04-08] Axios/UNC1069 - actor expande targeting a Lodash, Fastify, dotenv, mocha

- **Source**: [The Hacker News (3 abr 2026)](https://thehackernews.com/2026/04/unc1069-social-engineering-of-axios.html) / [Elastic Security Labs](https://www.elastic.co/security-labs/axios-one-rat-to-rule-them-all) / [Google Cloud Threat Intel](https://cloud.google.com/blog/topics/threat-intelligence/north-korea-threat-actor-targets-axios-npm-package)
- **Confidence**: Alta
- **What changes**: Extiende la entrada del 2026-04-06 sobre el ataque a Axios. Nuevos detalles publicados el 3 de abril confirman que UNC1069/Sapphire Sleet no se limitó a Axios: el mismo actor realizó campañas de social engineering paralelas contra mantenedores de **Lodash, Fastify, dotenv, mocha** y colaboradores del core de Node.js. Vector de ataque documentado: llamadas falsas de Microsoft Teams con error de "system update", luego lures en LinkedIn posando como "Openfort", y prompts falsos de Streamyard. El malware identificado es **WAVESHAPER.V2**, dependencia maliciosa `plain-crypto-js`, postinstall hook contactando `sfrclak[.]com:8000` para payloads de stage 2.
- **Action required**: Urgente
- **Details**: Si tu stack incluye Lodash, Fastify, dotenv o mocha, verificar también esas dependencias en el período del 31 de marzo 2026. IOCs adicionales: dominio C2 `sfrclak[.]com:8000`, paquete npm `plain-crypto-js`. El alcance del ataque es significativamente mayor de lo que se reportó inicialmente — no limitado a Axios.
