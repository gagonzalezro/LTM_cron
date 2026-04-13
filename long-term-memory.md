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

---

## [2026-04-08] Anthropic Claude Mythos Preview + Project Glasswing - modelo más potente, solo acceso restringido

- **Source**: [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) / [CNBC](https://www.cnbc.com/2026/04/07/anthropic-claude-mythos-ai-hackers-cyberattacks.html) / [VentureBeat](https://venturebeat.com/technology/anthropic-says-its-most-powerful-ai-cyber-model-is-too-dangerous-to-release) / [CyberScoop](https://cyberscoop.com/project-glasswing-anthropic-ai-open-source-software-vulnerabilities/) / [Anthropic API Release Notes](https://platform.claude.com/docs/en/release-notes/api)
- **Confidence**: Alta
- **What changes**: Anthropic anunció el 7 de abril 2026 **Claude Mythos Preview**, descrito como su "modelo frontera más poderoso hasta la fecha". **No se lanzará públicamente** por riesgo de ciberseguridad. Acceso exclusivo vía **Project Glasswing** (iniciativa de ciberseguridad defensiva con 12+ socios: AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, Linux Foundation, Microsoft, NVIDIA, Palo Alto Networks + 40 organizaciones). Benchmarks confirmados: SWE-bench Verified 93.9%, GPQA Diamond 94.6%, CyberGym 83.1%, Terminal-Bench 2.0 82.0%, USAMO 2026 97.6%. Demostración: encontró un bug de 27 años en OpenBSD y uno de 16 años en FFmpeg que evadió 5M tests automáticos.
- **Action required**: Monitorear
- **Details**: Anthropic comprometió $100M en usage credits a los participantes, $4M en financiamiento a open-source (Alpha-Omega/OpenSSF y Apache Foundation). Precio restringido: $25/M input, $125/M output (5x Opus 4.6). Disponible en Claude API, Bedrock, Vertex AI y Microsoft Foundry (todos con acceso por invitación). Ventana de disclosure público de vulnerabilidades: 90 días desde el 7 de abril. Para equipos con necesidades de seguridad avanzada: contactar a Anthropic para acceso a Project Glasswing.

---

## [2026-04-08] Claude Code v2.1.94 - cambio de comportamiento: effort level default ahora es HIGH

- **Source**: [Claude Code GitHub Releases](https://github.com/anthropics/claude-code/releases)
- **Confidence**: Alta
- **What changes**: Claude Code v2.1.94 (7 abril 2026) cambió el **effort level por defecto de `medium` a `high`** para usuarios con API key propia, Bedrock/Vertex/Foundry, Team y Enterprise. Esto aumenta la calidad de respuestas pero también el costo por request y la latencia. Otras features: soporte de Amazon Bedrock via Mantle (`CLAUDE_CODE_USE_MANTLE=1`), headers compactos de Slack con links en MCP, campo `keep-coding-instructions` en frontmatter, campo `hookSpecificOutput.sessionTitle` en hooks `UserPromptSubmit`. Fixes: agentes bloqueados tras 429s, login en macOS con keychain bloqueado, hooks en YAML frontmatter ignorados.
- **Action required**: Monitorear
- **Details**: El cambio de effort level es el más impactante operativamente: **revisar costos de API** si usas Claude Code v2.1.94+. Si se necesita el comportamiento anterior (medium), se puede configurar manualmente via settings. `v2.1.96` (8 abril) incluye un fix para requests a Bedrock fallando con 403 "Authorization header is missing" cuando se usaban `AWS_BEARER_TOKEN_BEDROCK` o `CLAUDE_CODE_SKIP_BEDROCK_AUTH` (regresión de 2.1.94).

---

## [2026-04-08] GitHub Copilot CLI - BYOK + modelos locales + deprecaciones GPT-5.1 Codex

- **Source**: [GitHub Changelog - April 2026](https://github.blog/changelog/month/04-2026/)
- **Confidence**: Alta
- **What changes**: Tres cambios en el período 1-7 abril 2026: (1) **Copilot CLI BYOK + local models** (7 abril): Copilot CLI permite conectar tu propio proveedor de modelo o ejecutar modelos **completamente locales** en lugar de routing a servidores de GitHub. (2) **GPT-5.1 Codex, GPT-5.1-Codex-Max, GPT-5.1-Codex-Mini deprecated** efectivo 1 abril 2026 — cualquier integración Copilot que especifique explícitamente esos model IDs falla. (3) **Code Review Metrics** (6 abril): admins de org pueden distinguir engagement activo vs. pasivo con Copilot code review en usage reports.
- **Action required**: Monitorear
- **Details**: El BYOK + local models es relevante para equipos con restricciones de privacidad de código (legal, finanzas, govtech) que antes no podían usar Copilot CLI. La deprecación de GPT-5.1 Codex es urgente si hay integraciones programáticas que especifiquen esos model IDs.

---

## [2026-04-08] Gemma 4 open model - Apache 2.0, 256K contexto, multimodal (Google, 2 abril 2026)

- **Source**: [Google Blog - Gemma 4](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/) / [Android Developers Blog - AICore](https://android-developers.googleblog.com/2026/04/AI-Core-Developer-Preview.html)
- **Confidence**: Alta
- **What changes**: Google lanzó **Gemma 4** el 2 de abril 2026 como modelo open-weight con licencia Apache 2.0. Cuatro variantes: E2B, E4B, **26B MoE**, **31B Dense**. Capacidades: 256K contexto, 140+ idiomas, multimodal (imágenes + texto), tool-calling. Disponible en Hugging Face, Kaggle, Google AI Studio. También: **AICore Developer Preview** para Android — código escrito para Gemma 4 se ejecuta automáticamente en dispositivos Gemini Nano 4; **AI Edge Gallery** para iPhone ejecuta Gemma 4 localmente.
- **Action required**: Monitorear
- **Details**: Las variantes 26B MoE y 31B Dense de Gemma 4 ya están disponibles en Vercel AI Gateway (ver entrada anterior). La licencia Apache 2.0 permite uso comercial sin restricciones. Para equipos que buscan modelos open-weight como alternativa a APIs propietarias: Gemma 4 con 256K contexto y multimodal es el estado del arte en modelos open-weight disponibles hoy.

---

## [2026-04-08] Anthropic Messages API en Amazon Bedrock - research preview, endpoint nativo

- **Source**: [Anthropic API Release Notes - 7 abr 2026](https://platform.claude.com/docs/en/release-notes/api) / [AWS What's New](https://aws.amazon.com/about-aws/whats-new/2026/04/amazon-bedrock-claude-mythos/)
- **Confidence**: Alta
- **What changes**: Anthropic lanzó el 7 de abril 2026 un **Messages API nativo en Amazon Bedrock** (research preview) en `us-east-1`. Endpoint: `/anthropic/v1/messages` — **idéntico en forma de request a la API de primera parte de Anthropic**. Características: zero operator access (AWS gestiona toda la infraestructura), sin necesidad de adaptar código entre la API de Anthropic y Bedrock.
- **Action required**: Monitorear
- **Details**: Para equipos que usan tanto la Claude API directa como Bedrock: este endpoint elimina la necesidad de wrappers o adaptadores. Acceso por solicitud al account executive de Anthropic. Complementa el soporte de Bedrock via Mantle en Claude Code v2.1.94.

---

## [2026-04-08] CVE-2026-34040 - Docker Engine: AuthZ plugin bypass via body truncation (CVSS 8.8)

- **Source**: [Docker official blog](https://www.docker.com/blog/docker-security-advisory-docker-engine-authz-plugin/) / [The Hacker News](https://thehackernews.com/2026/04/docker-cve-2026-34040-lets-attackers.html) / [Cyera Research](https://www.cyera.com/blog/cyera-research-discovers-docker-authorization-bypass-that-silently-disables-security-policies)
- **Confidence**: Alta
- **What changes**: Requests a la API de Docker >1 MB son silenciosamente truncados antes de llegar a los AuthZ plugins (OPA, Prisma Cloud, plugins custom), pero el daemon procesa el request completo. Permite creación de contenedores privilegiados y acceso al filesystem del host. Regresión de CVE-2024-41110. Revelado el 7 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar Docker Engine a `29.3.1` / Docker Desktop a `4.66.1`. Afecta cualquier deployment que use AuthZ plugins para control de acceso — si no se usan plugins de autorización, el riesgo es bajo. Docker EE/Mirantis: verificar actualizaciones del vendor.

---

## [2026-04-08] CVE-2026-33105 - Azure Kubernetes Service: escalada de privilegios sin autenticación (CVSS 10.0)

- **Source**: [TheHackerWire](https://www.thehackerwire.com/azure-kubernetes-service-critical-privilege-escalation-cve-2026-33105/) / [CVEDetails](https://www.cvedetails.com/cve/CVE-2026-33105/) / [CIRCL](https://vulnerability.circl.lu/vuln/cve-2026-33105)
- **Confidence**: Alta
- **What changes**: Un atacante de red sin autenticación puede eludir los controles RBAC y escalar privilegios hasta obtener control de workloads en AKS. Revelado el 3 de abril de 2026. Patch disponible via Azure Update Manager desde el mismo día.
- **Action required**: Urgente
- **Details**: Aplicar el patch de seguridad de abril 2026 via Azure Update Manager inmediatamente. Verificar todos los clusters AKS. El vector no requiere autenticación previa — la exposición desde red externa es posible.

---

## [2026-04-08] CVE-2026-34197 - Apache ActiveMQ Classic: RCE via Jolokia JMX-HTTP bridge (CVSS 8.8)

- **Source**: [Horizon3.ai](https://horizon3.ai/attack-research/disclosures/cve-2026-34197-activemq-rce-jolokia/) / [CVEFeed NVD](https://cvefeed.io/vuln/detail/CVE-2026-34197) / [TheHackerWire](https://www.thehackerwire.com/apache-activemq-code-injection-via-jolokia-jmx-http-bridge/)
- **Confidence**: Alta
- **What changes**: Atacante invoca `BrokerService.addNetworkConnector()` via Jolokia en `/api/jolokia/` con una URI de discovery maliciosa, disparando carga de un contexto Spring XML remoto y ejecución arbitraria de comandos OS. En versiones 6.0.0–6.1.1 es efectivamente sin autenticación (CVE-2024-32114). Revelado el 7 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a ActiveMQ Classic `5.19.4` o `6.2.3`. CVEs previos de ActiveMQ han sido explotados activamente por ransomware — el riesgo de explotación activa es alto. Deshabilitar el endpoint Jolokia (`/api/jolokia/`) si no es necesario como mitigación inmediata.

---

## [2026-04-08] CVE-2026-34208 - @nyariv/sandboxjs: sandbox escape por prototype pollution (CVSS 10.0)

- **Source**: [DEV.to/ThreatChain](https://dev.to/threatchain/cve-2026-34208-javascript-sandbox-library-cant-keep-attackers-out-5li) / [TheHackerWire](https://www.thehackerwire.com/vulnerability/CVE-2026-34208/) / [THREATINT](https://cve.threatint.eu/CVE/CVE-2026-34208)
- **Confidence**: Alta
- **What changes**: Constructor prototype pollution en `@nyariv/sandboxjs` (<0.8.36) permite que código dentro del sandbox escriba propiedades arbitrarias en globals del host, persistiendo across all sandbox instances del proceso. Revelado el 6 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a `@nyariv/sandboxjs >= 0.8.36`. Si se usa esta librería para ejecutar código de usuarios no confiables, tratar cualquier ejecución previa como potencialmente comprometida. No hay workaround viable — solo el upgrade soluciona el problema.

---

## [2026-04-08] CVE-2026-33744 - BentoML <1.4.37: command injection en bentofile.yaml (CVSS 7.8)

- **Source**: [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2026-33744/)
- **Confidence**: Media
- **What changes**: El campo `docker.system_packages` en `bentofile.yaml` se interpola directamente en comandos Dockerfile `RUN` sin sanitización. Permite RCE durante el build del contenedor en pipelines CI/CD que procesen configs no confiables. Revelado el 3 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar BentoML a `>=1.4.37` (el fix introduce escaping con `shlex`). El riesgo es mayor en plataformas MLOps donde usuarios externos pueden subir bentofiles. Requiere `env_manager=LOCAL` para ser explotable.

---

## [2026-04-08] 36 paquetes npm maliciosos (strapi-plugin-*): ataque quirúrgico a exchange cripto

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/36-malicious-npm-packages-exploited.html) / [CyberSecurityNews](https://cybersecuritynews.com/36-malicious-npm-strapi-packages/) / [SecurityWeek](https://www.securityweek.com/guardarian-users-targeted-with-malicious-strapi-npm-packages/)
- **Confidence**: Alta
- **What changes**: Descubierto el 5 de abril de 2026. 36 paquetes `strapi-plugin-*` publicados por 4 cuentas sock-puppet. 8 variantes de payload evolucionadas en 13 horas: desde RCE Redis y Docker container escape hasta explotación directa de PostgreSQL e implantes C2 persistentes. La variante final (v3.6.8) solo se activa si `hostname === 'prod-strapi'` — ataque dirigido a Guardarian (exchange de criptomonedas). Distinto del ataque Axios/UNC1069 documentado previamente.
- **Action required**: Urgente
- **Details**: Auditar `package.json` por paquetes `strapi-plugin-*` de publishers desconocidos. IOCs: cuentas npm `umarbek1233`, `kekylf12`, `tikeqemif26`, `umar_bektembiev1`; archivo `.node_gc.js` en `/tmp/` reinstalado cada minuto via cron; directorio `.truffler-cache/` en `node_modules`. Los scripts `postinstall` ejecutan el payload automáticamente al instalar.

---

## [2026-04-08] Go 1.26.2 / 1.25.9 - security releases (CVE-2026-32288, CVE-2026-32283)

- **Source**: [Go Release History](https://go.dev/doc/devel/release) / [golang-announce](https://groups.google.com/g/golang-announce/c/0uYbvbPZRWU) / [TechRepublic](https://www.techrepublic.com/article/news-golang-patches-security-flaws/)
- **Confidence**: Alta
- **What changes**: Publicados el 7 de abril de 2026. (1) CVE-2026-32288 (`archive/tar`): unbounded memory allocation al leer archivos TAR maliciosos con sparse map formato GNU antiguo — DoS/OOM. (2) CVE-2026-32283 (`crypto/tls`): deadlock DoS si múltiples key-update messages llegan en un solo post-handshake record en TLS 1.3. Fixes adicionales en `crypto/x509`, `html/template`, `os`, y el compilador.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a Go 1.26.2 (rama 1.26.x) o 1.25.9 (rama 1.25.x). El CVE-2026-32288 afecta cualquier servicio que procese archivos TAR desde fuentes no confiables. El CVE-2026-32283 afecta servidores TLS 1.3 expuestos a internet.

---

## [2026-04-08] Kubernetes ingress-nginx: EOL definitivo 24 marzo 2026 — migrar a Gateway API

- **Source**: [Kubernetes Blog](https://kubernetes.io/blog/2025/11/11/ingress-nginx-retirement/) / [ingress2gateway 1.0](https://kubernetes.io/blog/2026/03/20/ingress2gateway-1-0-release/)
- **Confidence**: Alta
- **What changes**: Desde el 24 de marzo de 2026, el repo `kubernetes/ingress-nginx` es read-only. Sin más releases, security patches ni bug fixes. **No confundir** con F5/NGINX Inc.'s `nginx-ingress` — ese sigue activo. La herramienta de migración `ingress2gateway` 1.0 fue lanzada el 20 de marzo de 2026.
- **Action required**: Migrar
- **Details**: Deployments existentes siguen funcionando pero quedarán expuestos a CVEs sin parche indefinidamente. `ingress2gateway` 1.0 soporta 30+ annotations y genera recursos Gateway API. Alternativas mantenidas: Traefik, HAProxy Ingress, Kong, Envoy/Contour. Planificar migración urgente.

---

## [2026-04-08] Kubernetes v1.36 (release 22 abril): breaking changes confirmados

- **Source**: [Kubernetes Blog Sneak Peek](https://kubernetes.io/blog/2026/03/30/kubernetes-v1-36-sneak-peek/)
- **Confidence**: Alta
- **What changes**: Release el 22 de abril de 2026 (14 días). Breaking changes: (1) **`gitRepo` volume driver eliminado** (deprecado desde v1.11) — workloads que lo usen fallarán; migrar a init containers o git-sync. (2) **HPA Scale to Zero habilitado por defecto** (`HPAScaleToZero` GA) — puede afectar autoscaling inesperadamente. (3) **`spec.externalIPs` en Services deprecado** (CVE-2020-8554, vector MITM); eliminación en v1.43. (4) **Modo IPVS en kube-proxy deprecado**.
- **Action required**: Monitorear
- **Details**: Verificar `gitRepo` volume usage con `kubectl get pods --all-namespaces -o yaml | grep gitRepo` antes del 22 de abril. El HPA Scale to Zero puede causar cold starts en workloads que no se esperan escalar a cero. Planificar migración de IPVS y `externalIPs`.

---

## [2026-04-08] Shopify April 2026: breaking changes live ahora — Scripts EOL 30 junio

- **Source**: [Weaverse Blog](https://weaverse.io/blogs/shopify-developer-breaking-changes-april-2026) / [Shopify Changelog](https://changelog.shopify.com/posts/shopify-scripts-can-no-longer-be-edited-or-published)
- **Confidence**: Alta
- **What changes**: Cuatro cambios con fechas concretas para apps Shopify/Hydrogen: (1) **Tokens expirantes obligatorios** (activo desde 1 abril): nuevas apps públicas solo pueden usar expiring offline access tokens. (2) **JSON metafield cap 128KB** en API 2026-04. (3) **Shopify Scripts freeze** (15 abril): no se puede editar ni publicar nuevos Scripts; se detienen definitivamente el **30 junio 2026** — migrar a Shopify Functions. (4) **CLI `--force` eliminado** (mayo 2026): reemplazar por `--allow-updates`/`--allow-deletes` en pipelines CI/CD.
- **Action required**: Migrar
- **Details**: El deadline de Shopify Scripts es el más urgente: 15 abril para dejar de editar, 30 junio para que dejen de ejecutarse. Auditar todos los Scripts activos y planificar migración a Shopify Functions ahora.

---

## [2026-04-08] Apple App Store: Xcode 26 e iOS 26 SDK obligatorios desde 28 abril 2026

- **Source**: [Apple Developer - Upcoming Requirements](https://developer.apple.com/news/upcoming-requirements/) / [9to5Mac](https://9to5mac.com/2026/02/03/apple-to-update-minimum-sdk-requirements-for-all-app-store-submissions/)
- **Confidence**: Alta
- **What changes**: Desde el 28 de abril de 2026 (20 días), todas las nuevas submissions al App Store deben compilarse con Xcode 26 y los SDKs de iOS 26/iPadOS 26/tvOS 26/visionOS 26/watchOS 26. Side effect: apps compiladas con iOS 26 SDK adoptan el estilo visual "Liquid Glass" en componentes nativos UI por defecto — puede cambiar el aspecto visual de la app.
- **Action required**: Urgente
- **Details**: Actualizar el entorno CI/CD a Xcode 26 antes del 28 de abril. Revisar visualmente la UI de la app compilada con el SDK de iOS 26 para detectar cambios por "Liquid Glass" no deseados. No cambia el requisito mínimo de iOS en runtime para usuarios finales.

---

## [2026-04-08] OpenAI Codex: pay-as-you-go + GPT-5.4 Tool Search para MCP/agentes con muchas tools

- **Source**: [OpenAI Codex flexible pricing](https://openai.com/index/codex-flexible-pricing-for-teams/) / [gHacks](https://www.ghacks.net/2026/04/03/openai-adds-pay-as-you-go-codex-seats-for-chatgpt-business-and-enterprise-teams/) / [OpenAI Tool Search Docs](https://developers.openai.com/api/docs/guides/tools-tool-search)
- **Confidence**: Alta
- **What changes**: Dos cambios en OpenAI no cubiertos en entradas anteriores: (1) **Codex pay-as-you-go** (3 abril): seats Codex-only sin coste fijo, billing por tokens; ChatGPT Business bajó de $25 a $20/mes/seat; promo $500 en créditos por workspace nuevo. (2) **GPT-5.4 Tool Search** en Responses API: el modelo ve solo nombres/descripciones de tools al inicio y recupera los schemas completos on-demand. Reduce significativamente el consumo de tokens en sistemas con muchas tools (MCP servers, agentes con 100+ tools).
- **Action required**: Monitorear
- **Details**: El Tool Search solo está disponible en `gpt-5.4`, `gpt-5.4-mini` y superiores (no en `gpt-5.4-nano`). Para integraciones programáticas de Codex: los model IDs `gpt-5.2-codex`, `gpt-5.1-codex*`, `gpt-5.1`, `gpt-5` se eliminan del Codex for ChatGPT el **14 de abril** — auditar integraciones con urgencia.

---

## [2026-04-08] Cohere Transcribe: ASR open-source Apache 2.0, lidera HF Open ASR Leaderboard (WER 5.42)

- **Source**: [Cohere Blog](https://cohere.com/blog/transcribe) / [TechCrunch](https://techcrunch.com/2026/03/26/cohere-launches-an-open-source-voice-model-specifically-for-transcription/) / [Hugging Face](https://huggingface.co/blog/CohereLabs/cohere-transcribe-03-2026-release)
- **Confidence**: Alta
- **What changes**: Cohere lanzó el 26 de marzo de 2026 `cohere-transcribe-03-2026`, modelo ASR de 2B parámetros con licencia Apache 2.0. Lidera el Open ASR Leaderboard de Hugging Face (WER 5.42) en 14 idiomas. Auto-hostable en GPUs consumer. Disponible gratis via Cohere API.
- **Action required**: Monitorear
- **Details**: Alternativa open-weight al estado del arte en speech-to-text (Whisper, AssemblyAI). Apache 2.0 permite uso comercial sin restricciones. Los 14 idiomas soportados incluyen inglés, español, francés, alemán, japonés y chino. Para equipos que buscan ASR auto-hospedado o gratuito, es el nuevo referente open-source.

---

## [2026-04-08] Rust 1.94.1 - CVE-2026-33056: symlink attack en Cargo via tar-rs

- **Source**: [Rust Blog - CVE-2026-33056](https://blog.rust-lang.org/2026/03/21/cve-2026-33056/) / [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-33056)
- **Confidence**: Alta
- **What changes**: La crate `tar-rs` usada internamente por Cargo seguía symlinks durante extracción de crates, permitiendo a una crate maliciosa hacer `chmod` de directorios arbitrarios fuera del root de extracción. Publicado el 21 de marzo, fix en Rust 1.94.1 (26 marzo 2026) actualizando `tar` a 0.4.45.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a Rust >= 1.94.1. Usuarios de registros alternos (no crates.io) no están protegidos automáticamente con versiones antiguas de Cargo — contactar al vendor del registry. crates.io fue auditado y confirmado limpio.

---

## [2026-04-08] Python 3.13.13 + 3.15.0a8 final alpha (7 abril 2026)

- **Source**: [Python.org - disponibilidad simultánea](https://discuss.python.org/t/python-3-15-0a8-3-14-4-and-3-13-13-are-now-available/106887) / [Python 3.15.0a8](https://www.python.org/downloads/release/python-3150a8/)
- **Confidence**: Alta
- **What changes**: Publicados el 7 de abril junto con Python 3.14.4 (ya documentado). (1) **Python 3.13.13**: 13ª release de mantenimiento, ~200 bugfixes desde 3.13.12. (2) **Python 3.15.0a8**: último alpha planificado antes de la beta. Features preview notables: UTF-8 como encoding por defecto en todo (PEP 686, breaking para proyectos legacy con encodings no-UTF-8), JIT compiler con +6-7% speedup en x86-64 y +12-13% en AArch64 (PEP 744), profiler de sampling estadístico (PEP 799), unpacking `*`/`**` en comprehensions (PEP 798).
- **Action required**: Monitorear
- **Details**: Python 3.13.13 es actualización de mantenimiento segura. El alpha 3.15 no debe usarse en producción, pero el cambio de encoding UTF-8 por defecto (PEP 686) es un breaking change para proyectos con archivos legacy en Latin-1/CP1252 — comenzar a auditar ahora antes de la release final (~octubre 2026).

---

## [2026-04-08] CVE-2025-59528 - Flowise AI: RCE crítico CVSS 10.0, explotación activa desde 7 abril

- **Source**: [BleepingComputer](https://www.bleepingcomputer.com/news/security/max-severity-flowise-rce-vulnerability-now-exploited-in-attacks/) / [The Hacker News](https://thehackernews.com/2026/04/flowise-ai-agent-builder-under-active.html) / [Security Affairs](https://securityaffairs.com/190471/security/attackers-exploit-critical-flowise-flaw-cve-2025-59528-for-remote-code-execution.html)
- **Confidence**: Alta
- **What changes**: CVE-2025-59528 (CVSS 10.0, divulgado en septiembre 2025) entró en explotación activa el 7 de abril de 2026, detección confirmada por VulnCheck Canary network. El nodo `CustomMCP` evalúa `mcpServerConfig` como JavaScript crudo sin sanitización, dando acceso completo al runtime de Node.js (incluyendo `child_process` y `fs`). 12.000–15.000 instancias Flowise expuestas en internet. Dos CVEs adicionales (CVE-2025-8943, CVE-2025-26319) también bajo explotación activa.
- **Action required**: Urgente
- **Details**: Actualizar a Flowise `>=3.1.1` (mínimo: 3.0.6 donde se aplicó el parche inicial). No existe workaround viable sin actualizar. Buscar instancias accesibles públicamente con `shodan search "flowise"` o similar. Para equipos que usen Flowise en CI/CD o como herramienta interna: tratar cualquier instancia accesible como comprometida hasta verificar la versión.

---

## [2026-04-08] CVE-2026-0621 - @modelcontextprotocol/sdk: ReDoS CVSS 8.7 en UriTemplate

- **Source**: [GitHub Advisory GHSA-8r9q-7v3j-jr4g](https://github.com/advisories/GHSA-8r9q-7v3j-jr4g) / [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-0621) / [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/@modelcontextprotocol/sdk/CVE-2026-0621/)
- **Confidence**: Alta
- **What changes**: La función `partToRegExp()` en la clase `UriTemplate` del SDK de MCP genera expresiones regulares con cuantificadores anidados al procesar variables exploded (`{/id*}`, `{?tags*}`), causando backtracking catastrófico. Un atacante envía una URI maliciosa via `resources/read` para causar 100% de CPU y DoS de todos los clientes. Vector de red, sin autenticación ni interacción de usuario.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `@modelcontextprotocol/sdk` a `>=1.25.2` (fix commit b392f02). Afecta cualquier MCP server que acepte requests de clientes no confiables. EPSS 0.023% (sin explotación activa conocida). Revisar si otras librerías de MCP (Python, Java) tienen el mismo patrón.

---

## [2026-04-08] CVE-2026-35515 - @nestjs/core: SSE injection CVSS 7.2, sin autenticación requerida

- **Source**: [GitLab Advisory GHSA-36xv-jgw5-4q75](https://advisories.gitlab.com/pkg/npm/@nestjs/core/CVE-2026-35515/) / [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-35515)
- **Confidence**: Alta
- **What changes**: `SseStream._transform()` en `@nestjs/core` interpola directamente `message.type` y `message.id` en el protocolo SSE sin sanitizar caracteres de newline. Vectores de ataque: (1) event spoofing — forzar callbacks incorrectos en `EventSource.addEventListener()`; (2) data injection — inyectar payloads `data:` con XSS si el cliente renderiza SSE como HTML; (3) corrupción de `Last-Event-ID` para manipular reconexiones. Vector de red, sin autenticación, baja complejidad. Publicado el 6-7 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `@nestjs/core` a `>=11.1.18`. El fix (commit 83558ae) añade sanitización de newlines. Afecta cualquier endpoint NestJS que use SSE con datos controlados por el usuario en los campos `type` o `id`.

---

## [2026-04-09] CVE-2026-23869 "React2DoS" - DoS en React Server Components via Flight protocol deserialization (CVSS 7.5)

- **Source**: [Vercel Changelog](https://vercel.com/changelog/summary-of-cve-2026-23869) / [Security Boulevard](https://securityboulevard.com/2026/04/react2dos-cve-2026-23869-when-the-flight-protocol-crashes-at-takeoff/) / [Akamai Research](https://www.akamai.com/blog/security-research/cve-2026-23864-react-nextjs-denial-of-service)
- **Confidence**: Alta
- **What changes**: Divulgado el 8 de abril de 2026. Una HTTP request maliciosa al endpoint de Server Functions del App Router puede disparar CPU excesiva al deserializar el payload del Flight protocol, causando DoS. Cualquier atacante de red sin autenticación puede interrumpir el servicio. Afecta Next.js 13.x, 14.x, 15.x y 16.x con App Router. Es una vulnerabilidad DoS distinta de CVE-2025-55182 (RCE).
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a Next.js `15.5.15` (rama 15.x) o `16.2.3` (rama 16.x). Vercel desplegó reglas WAF automáticas para proyectos hospedados, pero **no sustituyen al parche**. Apps que no usen el App Router ni Server Functions no están afectadas. Apps en Pages Router están seguras.

---

## [2026-04-09] CVE-2026-34043 - serialize-javascript <7.0.5: CPU exhaustion DoS via array-like objects (CVSS 5.9)

- **Source**: [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/serialize-javascript/CVE-2026-34043/) / [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2026-34043/)
- **Confidence**: Alta
- **What changes**: Publicado el 27 de marzo de 2026. `serialize-javascript` <7.0.5 entra en bucle de CPU infinita al serializar objetos que heredan de `Array.prototype` con un campo `length` muy grande. Un atacante de red sin autenticación puede causar DoS (100% CPU) si puede enviar input a un endpoint que serialice datos. Es dependencia transitiva de `terser-webpack-plugin` — usada por webpack, react-scripts, Next.js y Vite.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `serialize-javascript` a `>=7.0.5`. Ejecutar `npm audit` para detectar si alguna dependencia transitiva está afectada. El riesgo es mayor en apps que serialicen input de usuarios no confiables con esta librería. CWE-400.

---

## [2026-04-09] CVE-2026-39888/39890/39891 - PraisonAI ≤1.5.114: múltiples RCE críticos (CVSS 9.9 / 9.8)

- **Source**: [GitLab Advisory CVE-2026-39888](https://advisories.gitlab.com/pkg/pypi/praisonaiagents/CVE-2026-39888/) / [TheHackerWire](https://www.thehackerwire.com/critical-praisonai-sandbox-escape-rce-cve-2026-39888/)
- **Confidence**: Alta
- **What changes**: Publicados el 8 de abril de 2026. Tres CVEs en el framework de agentes PraisonAI: (1) **CVE-2026-39888** (CVSS 9.9): sandbox escape via frame traversal — el blocklist AST del subprocess omite `__traceback__`, `tb_frame`, `f_back`, `f_builtins`, permitiendo acceso a los builtins reales del host y ejecución arbitraria. (2) **CVE-2026-39890** (CVSS 9.8): RCE via YAML deserialization en la carga de definiciones de agentes. (3) **CVE-2026-39891**: template injection via agent input.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `praisonaiagents` y `praisonai` a `>=1.5.115`. Si se usa PraisonAI para ejecutar código de usuarios no confiables en producción, tratar el entorno como potencialmente comprometido hasta actualizar.

---

## [2026-04-09] CVE-2026-39892 - Python cryptography 45.0.0–46.0.6: buffer overflow en Hash.update() (CVSS 5.3)

- **Source**: [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/cryptography/CVE-2026-39892/)
- **Confidence**: Alta
- **What changes**: Publicado el 8 de abril de 2026. En Python >3.11, pasar buffers no-contiguos (ej. slices inversos como `data[::-1]`) a APIs como `Hash.update()`, `HMAC.update()` y cifrados simétricos puede leer más allá del límite del buffer — DoS o potencial lectura de memoria adyacente. Solo afecta las versiones 45.0.0–46.0.6.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `cryptography` a `>=46.0.7`. El riesgo es bajo para code paths con buffers de orientación estándar. Mayor riesgo en servicios que procesen slices invertidos de datos de red. CWE-119.

---

## [2026-04-09] GlassWorm extiende targeting a npm: react-native packages comprometidos (marzo 2026)

- **Source**: [StepSecurity Blog](https://www.stepsecurity.io/blog/malicious-npm-releases-found-in-popular-react-native-packages---130k-monthly-downloads-compromised) / [Sonatype Blog](https://www.sonatype.com/blog/hijacked-npm-packages-deliver-malware-via-solana-linked-to-glassworm) / [CyberSecurityNews](https://cybersecuritynews.com/glassworm-hits-popular-react-native-packages/)
- **Confidence**: Alta
- **What changes**: Extiende la entrada del 2026-04-06 sobre GlassWorm (Open VSX). El 16-18 de marzo de 2026, GlassWorm comprometió cuentas npm y publicó versiones maliciosas de `react-native-country-select@0.3.91` y `react-native-international-phone-number@0.11.8` (más versiones 0.12.1–0.12.3 en días posteriores). Total: 134.887+ descargas mensuales combinadas. Mismo vector Solana blockchain como dead drop C2 que en el ataque Open VSX. Un script `install.js` descarga y ejecuta un payload de segunda etapa.
- **Action required**: Urgente
- **Details**: Verificar que ningún proyecto React Native use estas versiones comprometidas. Versiones afectadas: `react-native-country-select@0.3.91`, `react-native-international-phone-number@0.11.8, 0.12.1, 0.12.2, 0.12.3`. Los mantenedores publicaron versiones limpias tras el reporte. GlassWorm es el mismo actor que atacó extensiones Open VSX en enero-marzo 2026 — la campaña se expande activamente.

---

## [2026-04-08] CVE-2026-39406/39407 - Hono: path traversal via double-slash en serveStatic (CVSS 5.3)

- **Source**: [GitLab Advisory CVE-2026-39406](https://advisories.gitlab.com/pkg/npm/@hono/node-server/CVE-2026-39406/) / [GitLab Advisory CVE-2026-39407](https://advisories.gitlab.com/pkg/npm/hono/CVE-2026-39407/)
- **Confidence**: Alta
- **What changes**: El middleware `serveStatic` en Hono normaliza paths con doble slash (`//admin/file`) pero el router no los matchea, permitiendo eludir middleware de autorización basado en rutas. Un atacante accede a archivos estáticos "protegidos" con requests como `GET //admin/secret.json`. Ambos CVEs son la misma bug raíz en dos paquetes distintos. Publicados el 8 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `@hono/node-server` a `>=1.19.13` (CVE-2026-39406) y `hono` a `>=4.12.12` (CVE-2026-39407). El fix (commit 025c30f) normaliza los paths antes de pasarlos al router. Afecta únicamente aplicaciones que usen `serveStatic` con protección de autorización basada en path de ruta.

---

## [2026-04-08] CVE-2026-34765 - Electron: window.open() trust bypass CVSS 6.0 (fixed en 39.8.5/40.8.5/41.1.0)

- **Source**: [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-34765) / [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/electron/CVE-2026-34765/)
- **Confidence**: Alta
- **What changes**: El lookup de ventanas por nombre (`window.open(url, targetName)`) no está limitado al contexto del opener. Un renderer puede navegar una ventana hijo abierta por otro renderer si comparten el mismo `targetName` y la ventana hijo tiene `webPreferences` elevados (p.ej. `nodeIntegration: true`). CVSS 6.0 (Medium), CWE-668. Solo afecta apps con múltiples ventanas top-level de distintos niveles de confianza. Publicado el 7 de abril de 2026.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar Electron a `39.8.5`, `40.8.5`, `41.1.0`, o `42.0.0-alpha.5`. Mitigación adicional: usar `webPreferences.contextIsolation: true` y `sandbox: true` en todas las ventanas. Si la app usa múltiples top-level windows con trust mixto, auditar el uso de nombres de target en `window.open()`.

---

## [2026-04-08] CVE-2026-23869 - Next.js RSC: DoS CVSS 7.5 (versiones 13.x–16.x), WAF Vercel como mitigación

- **Source**: [Vercel Changelog](https://vercel.com/changelog) / [Vercel Weekly 2026-04-06](https://community.vercel.com/t/vercel-weekly-2026-04-06/37604)
- **Confidence**: Alta
- **What changes**: Nueva CVE de Next.js (CVSS 7.5, High) que afecta React Server Components en versiones 13.x hasta 16.x — causa denial of service. Complementa y es distinto de CVE-2026-27979 y CVE-2026-29057 ya documentados en la entrada de Next.js 16.2. Vercel desplegó reglas WAF protectoras el 8 de abril de 2026; las reglas WAF **no son sustituto** del upgrade.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar Next.js a `>=16.2` (que ya incluye los parches anteriores de seguridad). Complementa la entrada del 2026-04-06 sobre Next.js 16.2. Si estás en ramas legacy (14.x, 15.x), verificar los advisory channels de Next.js para patches específicos de esas ramas.

---

## [2026-04-08] VeraCrypt - Microsoft terminó cuenta de firma de drivers Windows sin previo aviso

- **Source**: [SourceForge Discussion (Mounir Idrassi, 30 mar 2026)](https://sourceforge.net/p/veracrypt/discussion/general/thread/9620d7a4b3/) / [404 Media](https://www.404media.co/microsoft-abruptly-terminates-veracrypt-account-halting-windows-updates/) / [Hacker News (958 pts)](https://news.ycombinator.com/item?id=46861008)
- **Confidence**: Alta
- **What changes**: El 30 de marzo de 2026, Microsoft terminó sin previo aviso la cuenta de desarrollador de Mounir Idrassi (creador de VeraCrypt) usada para firmar el bootloader y drivers de Windows. Sin resolución, no pueden publicarse nuevas releases de VeraCrypt para Windows firmadas. Linux y macOS no están afectados. La versión existente (1.26.24) sigue funcionando pero el certificado de firma expirará en el futuro.
- **Action required**: Monitorear
- **Details**: No es una CVE — no hay vulnerabilidad activa. El riesgo es operativo: equipos que dependan de actualizaciones de VeraCrypt en Windows quedan sin actualizaciones hasta que se resuelva la situación. Un empleado de Microsoft (Rafael Rivera) ofreció ayuda interna. Si VeraCrypt es parte de tu stack de compliance/cifrado en Windows, monitorear el SourceForge thread para resolución.

---

## [2026-04-09] Next.js 16 - breaking changes de upgrade: async params obligatorio, middleware→proxy, AMP eliminado

- **Source**: [Next.js 16 Blog](https://nextjs.org/blog/next-16) / [Upgrade Guide v16](https://nextjs.org/docs/app/guides/upgrading/version-16)
- **Confidence**: Alta
- **What changes**: Next.js 16 (upgrade desde v15) elimina compatibilidad temporal que v15 mantenía: (1) `params` y `searchParams` en layouts/pages/route handlers son 100% async — el acceso síncrono ya no funciona. (2) `middleware.ts` renombrado a `proxy.ts`; edge runtime **no soportado** en proxy (solo nodejs). (3) `next/link` prop `legacyBehavior` **eliminada**. (4) Soporte AMP **eliminado** completamente — todas las APIs y config AMP removidas. (5) `revalidateTag()` requiere segundo argumento con perfil `cacheLife` — forma single-arg deprecada. (6) React Compiler 1.0 ahora estable vía opción `reactCompiler` (antes bajo `experimental`).
- **Action required**: Migrar
- **Details**: El punto más crítico es el cambio async/sync de params: cualquier componente que acceda `params.id` directamente rompe — debe cambiarse a `const { id } = await params`. Usar codemod oficial: `npx @next/codemod@latest upgrade`. El renombrado de middleware a proxy afecta estructura de archivos y cualquier config de CI que referencie `middleware.ts`. Complementa entry 2026-04-06 sobre Next.js 16.2 (performance y CVEs).

---

## [2026-04-09] React Native 0.85 - nuevo Animation Backend por defecto, Jest preset a paquete separado

- **Source**: [React Native 0.85 Release Blog](https://reactnative.dev/blog/2026/04/07/react-native-0.85)
- **Confidence**: Alta
- **What changes**: React Native 0.85 (7 abril 2026): (1) Nuevo Animation Backend activo por defecto. (2) Jest preset movido a paquete dedicado `@react-native/jest-preset` — el preset anterior `react-native` falla. (3) `CatalystInstanceImpl` **eliminado** (estaba deprecado). (4) `ReactZIndexedViewGroup` y `UIManagerHelper` deprecados. (5) `AccessibilityInfo.setAccessibilityFocus` deprecado en favor de `AccessibilityInfo.sendAccessibilityEvent`.
- **Action required**: Monitorear
- **Details**: Solo relevante para proyectos React Native. El cambio de Jest preset requiere actualizar `jest.config.js`: reemplazar `preset: 'react-native'` por `preset: '@react-native/jest-preset'` e instalar el paquete. El nuevo Animation Backend puede producir diferencias de comportamiento en animaciones existentes — verificar en staging antes de release.

---

## [2026-04-09] Claude Code v2.1.97 - Focus view, permisos Bash endurecidos, fixes de MCP y /resume

- **Source**: [GitHub Releases - anthropics/claude-code v2.1.97](https://github.com/anthropics/claude-code/releases/tag/v2.1.97)
- **Confidence**: Alta
- **What changes**: Release del 8 de abril 2026 (~21:52 UTC). Features nuevas: (1) **Focus view** (`Ctrl+O`) en modo `NO_FLICKER` — muestra solo el prompt, un resumen de una línea de la herramienta con diffstats, y la respuesta final. (2) **`refreshInterval`** en status line settings para re-ejecutar el comando N segundos. (3) **`workspace.git_worktree`** en el JSON de entrada del status line (true cuando el directorio es un worktree enlazado). (4) Indicador **`● N running`** en `/agents` junto a tipos de agente con subagentes activos. (5) Syntax highlighting para archivos de política **Cedar** (`.cedar`, `.cedarpolicy`). Fixes críticos: permisos Bash endurecidos (env-var prefixes, network redirects); colisión de nombres de reglas de permiso con propiedades prototype de JavaScript (causaba `settings.json` ignorado en silencio); memory leak de MCP HTTP/SSE de ~50 MB/hr; retries 429 con backoff exponencial (antes agotaban todos los intentos en ~13 segundos); múltiples bugs de `/resume` incluyendo pérdida de caché y mensajes no guardados.
- **Action required**: Monitorear
- **Details**: El fix de colisión de nombres en `settings.json` es relevante para cualquiera con reglas de permisos nombradas como `toString`, `constructor`, `hasOwnProperty`, etc. — si settings parecían ignorados, este puede ser el motivo. El fix del leak de MCP (~50 MB/hr) es importante para sesiones largas con servidores MCP que reconectan. Actualizar a v2.1.97. Supercede la entrada anterior de v2.1.94/2.1.96.

---

## [2026-04-09] PraisonAI - cluster de 3 CVEs críticos/altos (CVSS 9.9, 9.8, 8.8), publicados 8 abril

- **Source**: [GitLab Advisory CVE-2026-39888](https://advisories.gitlab.com/pkg/pypi/praisonaiagents/CVE-2026-39888/) / [GitLab Advisory CVE-2026-39890](https://advisories.gitlab.com/pkg/pypi/praisonai/CVE-2026-39890/) / [GitLab Advisory CVE-2026-39891](https://advisories.gitlab.com/pkg/pypi/praisonai/CVE-2026-39891/) / [TheHackerWire](https://www.thehackerwire.com/critical-praisonai-sandbox-escape-rce-cve-2026-39888/)
- **Confidence**: Alta
- **What changes**: Tres CVEs publicados el 8 de abril de 2026 en el ecosistema PraisonAI (framework de multi-agentes Python): (1) **CVE-2026-39888** (CVSS 9.9, CRÍTICO) — sandbox escape en `praisonaiagents` via frame traversal. El sandbox de `execute_code()` en modo subprocess no bloquea `__traceback__`, `tb_frame`, `f_back` y `f_builtins`, permitiendo recuperar el dict real de builtins y ejecutar código arbitrario. Afecta `praisonaiagents < 1.5.115`. (2) **CVE-2026-39890** (CVSS 9.8, CRÍTICO) — RCE via YAML deserialization en `praisonai`. El parsing de YAML para definiciones de agente no deshabilita tags peligrosos (`!!js/function`, `!!js/undefined`), permitiendo embed y ejecución de JavaScript al cargar configs. Afecta `praisonai < 4.5.115`. (3) **CVE-2026-39891** (CVSS 8.8, ALTO) — template injection en tool definitions de agentes. Input de usuario no escapado en `agent.start()` es procesado como template expression. Afecta `praisonai < 4.5.115`. Las tres son explotables de forma remota sin autenticación.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `praisonaiagents` a `>=1.5.115` y `praisonai` a `>=4.5.115`. Si PraisonAI está expuesto a inputs no confiables (usuarios externos, uploads de YAML), tratar como urgente. CVE-2026-39888 es especialmente grave para plataformas MLOps que usen el sandbox de ejecución de código de PraisonAI.

---

## [2026-04-09] CVE-2026-39847 - Emmett (Python web framework): path traversal crítico en RSGI handler (CVSS 9.1)

- **Source**: [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-39847) / [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/emmett/CVE-2026-39847/)
- **Confidence**: Alta
- **What changes**: CVE-2026-39847 (CVSS 9.1, CWE-22) publicado el 7 de abril de 2026. El RSGI static handler de Emmett para rutas internas `/__emmett__/` no sanitiza secuencias `../`. Un atacante puede leer archivos arbitrarios del servidor usando requests como `GET /__emmett__/../etc/passwd`. Sin autenticación, acceso remoto, baja complejidad.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar Emmett a `>=2.8.1`. Afecta `emmett` 2.5.0 a 2.8.0. Framework Python de nicho pero con exposición crítica si está desplegado en producción. Si no es posible actualizar inmediatamente, bloquear el path `/__emmett__/` en el reverse proxy como mitigación temporal.

---

## [2026-04-09] CVE-2026-39892 - Python `cryptography`: buffer overflow en APIs de buffer (CVSS 5.3)

- **Source**: [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/cryptography/CVE-2026-39892/) / [Snyk](https://security.snyk.io/package/pip/cryptography)
- **Confidence**: Alta
- **What changes**: CVE-2026-39892 (CVSS 5.3, Medium) publicado el 8 de abril de 2026. La librería `cryptography` (PyPI) lee más allá del límite del buffer cuando se le pasan buffers no contiguos a APIs que aceptan Python buffers (p.ej. `Hash.update()` con un slice invertido en Python >3.11). Puede causar corrupción de memoria o DoS. Afecta versiones 45.0.0 a 46.0.6.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `cryptography` a `>=46.0.7`. El paquete `cryptography` es una dependencia transitiva de muchos proyectos Python (paramiko, pyOpenSSL, Twisted, requests con TLS, etc.) — ejecutar `pip show cryptography` para verificar la versión instalada. El riesgo es mayor si se procesan inputs no confiables en operaciones criptográficas con buffers.

---

## [2026-04-10] Anthropic Claude Managed Agents - public beta, nuevo producto de infraestructura de agentes

- **Source**: [Claude Blog oficial](https://claude.com/blog/claude-managed-agents) / [Docs: Overview](https://platform.claude.com/docs/en/managed-agents/overview) / [SiliconANGLE](https://siliconangle.com/2026/04/08/anthropic-launches-claude-managed-agents-speed-ai-agent-development/) / [InfoWorld](https://www.infoworld.com/article/4156852/anthropic-rolls-out-claude-managed-agents.html) / [HelpNetSecurity](https://www.helpnetsecurity.com/2026/04/09/claude-managed-agents-bring-execution-and-control-to-ai-agent-workflows/)
- **Confidence**: Alta
- **What changes**: Anthropic lanzó el 8 de abril de 2026 **Claude Managed Agents** en public beta — un harness de agentes completamente gestionado como servicio API. Permite correr Claude como agente autónomo sin gestionar infraestructura: sandboxing, checkpointing, credenciales, permisos y tracing end-to-end incluidos. Core concepts: **Agent** (modelo + system prompt + tools + MCP servers + skills), **Environment** (container cloud), **Session** (instancia de agente en ejecución), **Events** (SSE streaming bidireccional). Todos los endpoints requieren header beta `managed-agents-2026-04-01`. Herramientas built-in disponibles via `agent_toolset_20260401`: bash, file ops, web search, code execution. **Precios**: rates estándar de tokens Claude API + **$0.08/session-hora** de runtime activo; primeras 50 horas/día por organización gratuitas; web search: $10/1000 búsquedas. Early adopters: Notion, Rakuten, Sentry, Asana.
- **Action required**: Monitorear
- **Details**: Complementa Claude Code (agentic coding) y Claude Cowork (desktop + local files automation) como tercer pilar del ecosistema agéntico de Anthropic. Multi-agent coordination y self-evaluation están aún en research preview con acceso separado. Máximo 20 skills por sesión. El CLI `ant` (ver entrada siguiente) integra nativamente con Managed Agents. Anthropic afirma 10x mejora en tiempo de ship para early adopters. Relevante para cualquier equipo que esté construyendo agentes y quiera eliminar la complejidad de infraestructura.

---

## [2026-04-10] Anthropic ant CLI (`anthropic-cli`) - nuevo CLI oficial para la Claude API

- **Source**: [GitHub: anthropics/anthropic-cli](https://github.com/anthropics/anthropic-cli) / [Releasebot Anthropic](https://releasebot.io/updates/anthropic) / [blockchain.news análisis](https://blockchain.news/ainews/anthropic-launches-claude-managed-agents-build-and-deploy-via-console-claude-code-and-new-cli-2026-analysis)
- **Confidence**: Alta
- **What changes**: Anthropic publicó el 8 de abril de 2026 el repositorio oficial `anthropics/anthropic-cli`, un CLI en Go para interactuar con la Claude API. El binario se llama `ant`. Distinto de `claude` (Claude Code CLI) — `ant` es para gestión de recursos API y despliegue de agentes. Instalación: `go install 'github.com/anthropics/anthropic-cli/cmd/ant@latest'` (requiere Go ≥1.22). Capacidades principales: crear/gestionar mensajes y sesiones de Managed Agents, versionar recursos API en YAML (agentes, environments, skills), integración nativa con Claude Code, output en múltiples formatos (json, yaml, jsonl, pretty) y transformación con sintaxis GJSON.
- **Action required**: Monitorear
- **Details**: Especialmente útil para equipos que usen Managed Agents en CI/CD — permite definir agentes en YAML y versionarlos en el repo. Complementa el Console y Claude Code como tercera vía de deploy de agentes. El repositorio es nuevo (creado en torno al 8 abril 2026) y está en desarrollo activo. Requiere `ANTHROPIC_API_KEY` configurado.

---

## [2026-04-10] Claude Code v2.1.98 - Vertex AI wizard, Monitor tool, sandboxing en Linux

- **Source**: [claudeupdates.dev/version/2.1.98](https://www.claudeupdates.dev/version/2.1.98) / [claude-world.com](https://claude-world.com/articles/claude-code-2198-release/) / [GitHub Releases anthropics/claude-code](https://github.com/anthropics/claude-code/releases)
- **Confidence**: Alta
- **What changes**: Claude Code v2.1.98 (9 abril 2026, 57 cambios totales) añade sobre v2.1.97: (1) **Google Vertex AI setup wizard** — asistente interactivo desde la pantalla de login al seleccionar "3rd-party platform", guía autenticación GCP, proyecto/región, verificación de credenciales y pinning de modelo. (2) **Monitor tool** — permite hacer streaming de eventos desde scripts en background (complementa el Run in Background de v2.1.97). (3) **`CLAUDE_CODE_PERFORCE_MODE`** env var — Edit/Write/NotebookEdit fallan en archivos read-only con hint `p4 edit` en vez de sobreescribir silenciosamente (relevante para equipos con Perforce). (4) **Subprocess sandboxing con PID namespace isolation** en Linux cuando se activa `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`. (5) **`--exclude-dynamic-system-prompt-sections`** flag en print mode — mejora el cache cross-usuario de prompts del sistema.
- **Action required**: Monitorear
- **Details**: El Vertex AI wizard supercede la entrada anterior de v2.1.94 que introdujo el wizard para AWS Bedrock — ahora los tres providers principales (Anthropic, Bedrock, Vertex) tienen asistentes interactivos de setup. El Monitor tool es complementario al campo `run_in_background` del Bash tool. El sandboxing con PID namespace isolation es una mejora de seguridad significativa para entornos que ejecuten código no confiable. Actualizar a v2.1.98. Supercede la entrada de v2.1.97.

---

## [2026-04-10] CVE-2026-39860 - Nix: privilege escalation a root via symlink en multi-user installs (CVSS 9.0)

- **Source**: [NixOS Discourse — Security Advisory oficial](https://discourse.nixos.org/t/nix-security-advisory-privilege-escalation-via-symlink-following-during-fod-output-registration/76900) / [TheHackerWire](https://www.thehackerwire.com/nix-arbitrary-file-overwrite-to-root-cve-2026-39860/) / [THREATINT](https://cve.threatint.eu/CVE/CVE-2026-39860)
- **Confidence**: Alta
- **What changes**: CVE-2026-39860 (CVSS 9.0, CRÍTICO) publicado el 8 de abril de 2026. Es una regresión del fix de CVE-2024-27297. Durante el registro de outputs de derivaciones Fixed-Output (FOD), el proceso Nix (corriendo en el host mount namespace) sigue symlinks creados por el builder dentro del build chroot. Cualquier usuario del grupo `allowed-users` (por defecto todos los usuarios del sistema) puede crear un symlink hacia un path arbitrario del filesystem y hacer que el daemon Nix (root) sobreescriba ese archivo con el output de la derivación → **escalada de privilegios a root**. Solo afecta builds sandboxed en Linux; macOS no está afectado.
- **Action required**: Actualizar dependencia
- **Details**: Versiones con fix: **2.34.5, 2.33.4, 2.32.7, 2.31.4, 2.30.4, 2.29.3, 2.28.6**. Afecta a cualquier instalación multi-user de Nix en Linux (incluyendo NixOS). Los desarrolladores que usen Nix como gestor de paquetes en entornos de CI/CD compartidos o multi-usuario deben actualizar inmediatamente. Verificar versión con `nix --version`. GHSA: `GHSA-g3g9-5vj6-r3gj`.

---

## [2026-04-10] Vite 8.0 - Rolldown como único bundler (10-30x más rápido), breaking changes de configuración

- **Source**: [Vite 8.0 Blog oficial](https://vite.dev/blog/announcing-vite8) / [Migration Guide Vite 7→8](https://vite.dev/guide/migration) / [Vite Releases](https://vite.dev/releases)
- **Confidence**: Alta
- **What changes**: Vite 8.0 (12 marzo 2026) reemplaza el sistema dual esbuild/Rollup con **Rolldown** (bundler Rust, 10-30x más rápido). Breaking changes de configuración: (1) `build.rollupOptions` → **`build.rolldownOptions`**; (2) `worker.rollupOptions` → **`worker.rolldownOptions`**; (3) `optimizeDeps.esbuildOptions` → **`optimizeDeps.rolldownOptions`** (auto-converted temporalmente); (4) `esbuild` config → **`oxc`** config; (5) CSS minification ahora usa **Lightning CSS** por defecto en vez de esbuild; (6) JS minification usa **Oxc Minifier** en vez de esbuild; (7) CJS interop: el `default` import de módulos CJS se maneja de forma consistente (puede romper código que depende del comportamiento previo); (8) `import.meta.hot.accept()` ya no acepta URL — pasar id; (9) `output.manualChunks` en forma de objeto eliminado; (10) Formatos `'system'` y `'amd'` eliminados; (11) Target de browser mínimo aumentado: Chrome/Edge 107→111, Firefox 104→114, Safari 16.0→16.4. Requiere Node.js 20.19+ o 22.12+.
- **Action required**: Actualizar dependencia
- **Details**: Casos de rendimiento real: Linear 46s→6s, Beehiiv -64%, Ramp -57%, Mercedes-Benz.io -38% de build time. Migración recomendada para proyectos complejos: primero cambiar a `rolldown-vite` en Vite 7 para aislar issues de Rolldown, luego upgrade a Vite 8. Para la mayoría de proyectos el upgrade es suave gracias a la capa de compatibilidad que auto-convierte configs de esbuild/Rollup. Esto afecta directamente a proyectos React (CRA-reemplazos), Vue, Svelte, SvelteKit y cualquier app que use Vite como bundler. El punto (9) de CJS interop es el más silencioso y peligroso — testear con `legacy.inconsistentCjsInterop: true` para detectar regresiones.

---

## [2026-04-10] GitLab 18.10.3/18.9.5/18.8.9 - patch de seguridad con CVE-2026-5173 (CVSS 8.5) y DoS sin autenticación

- **Source**: [GitLab Patch Release 18.10.3 (oficial)](https://about.gitlab.com/releases/2026/04/08/patch-release-gitlab-18-10-3-released/) / [SecurityOnline](https://securityonline.info/gitlab-security-patch-18-10-3-websocket-graphql-vulnerabilities/) / [CyberPress](https://cyberpress.org/gitlab-fixes-critical-bugs/)
- **Confidence**: Alta
- **What changes**: GitLab publicó el 8 de abril de 2026 versiones parcheadas para CE y EE que corrigen **12 CVEs** incluyendo tres de alta severidad: (1) **CVE-2026-5173** (CVSS 8.5, HIGH) — método WebSocket expuesto permite a usuarios autenticados eludir controles de acceso e invocar métodos de servidor no intencionados; afecta versiones 16.9.6–18.10.2. (2) **CVE-2026-1092** (CVSS 7.5, HIGH) — DoS sin autenticación vía payloads JSON malformados en la Terraform state lock API; afecta desde 12.10. (3) **CVE-2025-12664** (CVSS 7.5, HIGH) — DoS sin autenticación vía consultas GraphQL repetidas; afecta desde 13.0. CVEs adicionales de severidad media (4.3-6.5): XSS en analytics dashboard, code injection en reports de calidad, divulgación de email, DoS en APIs de SBOM/CSV.
- **Action required**: Urgente
- **Details**: Versiones parcheadas: **18.10.3, 18.9.5, 18.8.9**. GitLab recomienda actualizar inmediatamente todas las instalaciones self-managed. Los CVEs de DoS sin autenticación (CVE-2026-1092 y CVE-2025-12664) son especialmente críticos para instancias expuestas a internet. CVE-2026-5173 requiere autenticación pero permite bypass de RBAC via WebSocket — relevante en entornos multi-tenant o multi-organización.

---

## [2026-04-10] Vercel - Claude Opus 4.6 Fast Mode en AI Gateway + GLM 5.1 + Sandbox CLI + Django zero-config

- **Source**: [Vercel Changelog — Opus 4.6 Fast Mode](https://vercel.com/changelog/opus-4-6-fast-mode-available-on-ai-gateway) / [Vercel Changelog](https://vercel.com/changelog)
- **Confidence**: Alta
- **What changes**: Cuatro actualizaciones de Vercel entre el 7-10 de abril de 2026: (1) **Claude Opus 4.6 Fast Mode en AI Gateway** (7 abril) — 2.5x más rápido en output tokens al mismo nivel de inteligencia, precio 6x la tarifa estándar de Opus 4.6. (2) **GLM 5.1 en AI Gateway** (7 abril) — modelo de Z.ai diseñado para tareas autónomas de larga duración. (3) **`vercel sandbox` CLI subcommand** (8 abril) — gestión de Vercel Sandbox desde la CLI sin necesidad del dashboard. (4) **Soporte Django zero-config** (9 abril) — apps Django se reconocen y despliegan automáticamente sin configuración manual de redirects; usa Fluid compute con Active CPU pricing. (5) **Anomaly alert configuration** (10 abril, mismo día) — Observability Plus ahora permite configurar alertas de anomalía con filtros por proyecto, métrica, HTTP status code y ruta; auto-investigación con routing a Slack/email.
- **Action required**: Monitorear
- **Details**: El Fast Mode de Opus 4.6 (punto 1) es relevante para workflows de human-in-the-loop donde la velocidad de respuesta importa más que el costo. A 6x el precio standard, evaluar el ROI antes de activar por defecto. El soporte Django (punto 4) expande Vercel más allá del ecosistema JS/Python asíncrono — equipos con backends Django pueden deployar sin configuración adicional. La anomaly alert (punto 5) complementa la entrada anterior sobre Observability Plus pricing.

---

## [2026-04-10] CVE-2026-34486 - Apache Tomcat: bypass de EncryptInterceptor, severidad "Important" (Tomcat 11.0.20)

- **Source**: [Apache Tomcat 11 Security Page](https://tomcat.apache.org/security-11.html) / [TheHackerWire CVE-2026-34486](https://www.thehackerwire.com/vulnerability/CVE-2026-34486/)
- **Confidence**: Alta
- **What changes**: CVE-2026-34486 fue divulgado públicamente el 9 de abril de 2026. Es una regresión introducida por el fix de CVE-2026-29146 (padding oracle attack en `EncryptInterceptor`) — el parche fue incompleto y permite eludir `EncryptInterceptor` completamente, exponiendo datos que deberían estar cifrados en tránsito entre nodos del cluster Tomcat. Afecta únicamente **Apache Tomcat 11.0.20** según la página oficial de seguridad (y posiblemente 10.1.53, 9.0.116 según otras fuentes). Severity: **Important** (clasificación Apache). No se ha publicado CVSS score.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a **Tomcat 11.0.21** (lanzado 4 abril 2026), y según fuentes secundarias también a 10.1.54 / 9.0.117. Solo afecta deployments que usen `EncryptInterceptor` para cifrado de sesiones distribuidas (clustering). Si no usas `EncryptInterceptor`, el riesgo es nulo. Verificar con `grep -r "EncryptInterceptor" conf/` en el directorio de Tomcat.

---

## [2026-04-10] CVE-2026-32871 - FastMCP Python: SSRF + path traversal en OpenAPIProvider (CVSS 10.0 CRITICAL)

- **Source**: [GitHub Advisory GHSA-vv7q-7jx5-f767](https://github.com/PrefectHQ/fastmcp/security/advisories/GHSA-vv7q-7jx5-f767) / [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/fastmcp/CVE-2026-32871/)
- **Confidence**: Alta
- **What changes**: CVE-2026-32871 (CVSS 10.0, CRITICAL, publicado 31 marzo 2026). El método `_build_url()` del `OpenAPIProvider` de FastMCP sustituye los parámetros de path directamente en la URL sin URL-encoding. Un atacante que controle un parámetro de path (ej. `user_id = "../../admin/secrets"`) puede usar path traversal para escapar del prefijo API y acceder a endpoints internos arbitrarios del backend — con las credenciales de autenticación del MCP provider (SSRF autenticado). Afecta `fastmcp < 3.2.0`.
- **Action required**: Urgente
- **Details**: Actualizar `fastmcp` a `>=3.2.0`. El fix aplica `urllib.parse.quote(str(param), safe="")` a todos los valores de parámetros de path. Crítico para cualquier MCP server construido con FastMCP que exponga operaciones OpenAPI con parámetros de path controlados por el usuario. Puede permitir: acceso a endpoints privados no expuestos en el spec OpenAPI, escalada de privilegios via credenciales heredadas, exfiltración de datos, y movimiento lateral dentro de redes internas.

---

## [2026-04-10] CVE-2026-39885 - mcp-from-openapi: SSRF + local file read via $ref dereferencing (CVSS 7.5 HIGH)

- **Source**: [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/mcp-from-openapi/CVE-2026-39885/) / [CVEReports](https://cvereports.com/reports/CVE-2026-39885)
- **Confidence**: Alta
- **What changes**: CVE-2026-39885 (CVSS 7.5, HIGH, publicado 8-9 abril 2026). `mcp-from-openapi` usa `@apidevtools/json-schema-ref-parser` para dereferencias `$ref` en specs OpenAPI sin configurar restricciones de URL ni resolvers custom. Una spec OpenAPI maliciosa con `$ref` apuntando a direcciones de red interna, endpoints de metadata cloud (ej. `http://169.254.169.254/latest/meta-data/`), o archivos locales (`file:///etc/passwd`) hace que la librería los fetche durante `initialize()`. Vector: red, sin autenticación, sin interacción de usuario. Afecta `mcp-from-openapi < 2.3.0` / `@frontmcp/sdk < 1.0.4`.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `@frontmcp/sdk` a `>=1.0.4` (que incluye `mcp-from-openapi >= 2.3.0`). El fix impone un blocklist de URLs por defecto. Especialmente crítico para aplicaciones que procesen specs OpenAPI de fuentes no confiables (usuarios externos, uploads de specs). La vulnerabilidad permite robo de credenciales cloud via IMDS (AWS/GCP/Azure) y lectura arbitraria de archivos del sistema.

---

## [2026-04-09] Anthropic Advisor Tool - patrón executor+advisor GA en public beta

- **Source**: [Anthropic API Release Notes (oficial)](https://platform.claude.com/docs/en/release-notes/overview) / [Claude Blog - The Advisor Strategy](https://claude.com/blog/the-advisor-strategy)
- **Confidence**: Alta
- **What changes**: Anthropic lanzó el 9 de abril 2026 el **advisor tool** en public beta. Permite que un modelo executor rápido y barato (Sonnet 4.6 o Haiku 4.5) consulte a Opus 4.6 como advisor mid-task dentro de **una única llamada a `/v1/messages`**, sin round-trips adicionales. El advisor accede al contexto compartido, devuelve un plan o corrección, y el executor reanuda. Beta header requerido: `anthropic-beta: advisor-tool-2026-03-01`. Tipo de tool en la API: `advisor_20260301`. Benchmarks confirmados: Sonnet+Opus advisor: 74.8% vs 72.1% en SWE-bench Multilingual (+2.7pp), -11.9% coste. Haiku+Opus advisor: 41.2% vs 19.7% en BrowseComp (+21.5pp), **85% más barato que Sonnet solo**.
- **Action required**: Monitorear
- **Details**: Usar `max_uses` para limitar el número de invocaciones al advisor y controlar el coste (los tokens del advisor se facturan a tarifas de Opus, los del executor a tarifas del modelo executor). El patrón es especialmente efectivo en flujos agenticos de larga duración donde la mayoría de pasos son rutinarios pero ocasionalmente se requiere razonamiento profundo. Implementación: añadir la tool `advisor_20260301` a la lista de tools en la request y el modelo la invocará automáticamente cuando lo necesite.

---

## [2026-04-10] Claude Code v2.1.101 - /team-onboarding, OS CA cert store, fix command injection en `which`

- **Source**: [GitHub Releases anthropics/claude-code v2.1.101](https://github.com/anthropics/claude-code/releases/tag/v2.1.101)
- **Confidence**: Alta
- **What changes**: Claude Code v2.1.101 (10 abril 2026, ~19:03 UTC). Features: (1) **`/team-onboarding`** — genera una guía de ramp-up para nuevos compañeros basada en el uso local de Claude Code (historial, patterns, setup). (2) **OS CA certificate store trust por defecto** — proxies TLS corporativos funcionan automáticamente; para usar solo CAs del bundle de Anthropic: `CLAUDE_CODE_CERT_STORE=bundled`. (3) `/ultraplan` y remote-session auto-crean entornos cloud por defecto. (4) Improved brief mode retry para respuestas plain-text. Fixes críticos: (5) **command injection vulnerability en fallback POSIX `which`** — fix de seguridad para detection de comandos en sistemas sin `which` en PATH estándar. (6) Memory leak en sesiones largas con copias de mensajes históricos. (7) Múltiples bugs de `/resume` picker y edición de archivos. Supercede entrada de v2.1.98.
- **Action required**: Monitorear
- **Details**: El fix de command injection en `which` (punto 5) es relevante para sistemas Linux/macOS con configuraciones de PATH no estándar. El trust del OS CA store (punto 2) elimina la necesidad de configuración manual de certificados en entornos corporativos con TLS inspection. Actualizar a v2.1.101.

---

## [2026-04-12] CVE-2026-34621 - Adobe Acrobat/Reader: zero-day activo (CVSS 8.6), explotado desde diciembre 2025

- **Source**: [The Hacker News - Adobe patches actively exploited Acrobat Reader flaw](https://thehackernews.com/2026/04/adobe-patches-actively-exploited.html) / [Adobe Security Bulletin APSB26-34](https://helpx.adobe.com/security/products/acrobat/apsb26-26.html)
- **Confidence**: Alta
- **What changes**: CVE-2026-34621 (CVSS 8.6, revisado desde 9.6) — **prototype pollution** en la capa JavaScript de Adobe Acrobat/Reader que resulta en **arbitrary code execution**. Parche publicado el 12 de abril de 2026 como actualización de emergencia. Explotación activa confirmada; evidencia de explotación desde **diciembre de 2025** (~4 meses antes del parche). Vector de ataque: local, requiere interacción del usuario (abrir un PDF malicioso). Afecta: Acrobat DC / Reader DC `≤26.001.21367`, Acrobat 2024 `≤24.001.30356` (Win) / `≤24.001.30360` (Mac).
- **Action required**: Urgente
- **Details**: Versiones parcheadas: Acrobat DC / Reader DC → `26.001.21411`; Acrobat 2024 → `24.001.30362` (Win) / `24.001.30360` (Mac). Actualizar inmediatamente via Help > Check for Updates. El ataque funciona mediante PDFs maliciosos que ejecutan JavaScript para envenenar el prototype del runtime de Acrobat. PDFs recibidos por email, Slack, o descargados de sitios no confiables son el vector principal. No afecta navegadores con PDF viewer propio (Chrome, Firefox, Edge) — solo la app de escritorio de Adobe Reader/Acrobat.

---

## [2026-04-06] OpenAI ChatGPT 5.5 + Super App desktop: unificación de ChatGPT, Codex y Atlas browser

- **Source**: [MacRumors - OpenAI Super App](https://www.macrumors.com/2026/03/20/openai-super-app-in-development-chatgpt/) / [The Decoder - OpenAI superapp](https://the-decoder.com/openai-plans-to-merge-chatgpt-codex-and-atlas-browser-into-a-single-desktop-superapp/) / [Neowin](https://www.neowin.net/news/openai-to-merge-atlas-browser-chatgpt-and-codex-into-a-single-desktop-super-app/)
- **Confidence**: Media
- **What changes**: OpenAI lanzó el 6 de abril de 2026 **ChatGPT 5.5** y una **Super App desktop unificada** que fusiona ChatGPT, Codex (agente de coding, reescrito en Rust) y Atlas (browser AI-powered) en un único cliente nativo. ChatGPT 5.5 es un update del modelo centrado en memory management, task continuity e instruction-following — bridge hacia GPT-6 "Spud" (aún en entrenamiento). El 9 de abril se incorporó **GPT-5.3 Instant Mini** como nuevo modelo fallback en ChatGPT (reemplaza GPT-5 Instant Mini, más natural y con mejor escritura). OpenAI también introdujo un nuevo tier de $100/mes/seat Pro (hasta 10x más Codex que Plus).
- **Action required**: Monitorear
- **Details**: Relevante para equipos que evalúen alternativas a Claude Code / Cursor para coding agéntico: Codex en la Super App es el competidor directo (escribe, testea y depura código de forma autónoma). La Super App está disponible primero para Plus ($20/mes) y Pro ($200/mes), con rollout limitado al tier gratuito. ChatGPT 5.5 ≠ GPT-5.5 "Spud" — este último completó pretraining el 24 mar 2026 y se espera en Q2 2026. El nuevo tier $100/mes Pro se sitúa entre Plus y Pro y enfatiza acceso extendido a Codex.

---

## [2026-04-10] fast-jwt: cluster de 3 CVEs en JWT verification (CVSS 9.1 / 5.3 / 4.2), publicados 3-9 abril

- **Source**: [GitLab CVE-2026-35039](https://advisories.gitlab.com/pkg/npm/fast-jwt/CVE-2026-35039/) / [GitLab CVE-2026-35040](https://advisories.gitlab.com/pkg/npm/fast-jwt/CVE-2026-35040/) / [GitLab CVE-2026-35041](https://advisories.gitlab.com/pkg/npm/fast-jwt/CVE-2026-35041/) / [CVEReports](https://cvereports.com/reports/CVE-2026-35041)
- **Confidence**: Alta
- **What changes**: Tres CVEs en `fast-jwt` publicados entre el 3 y 9 de abril de 2026: (1) **CVE-2026-35039** (CVSS 9.1, CRÍTICO, 3 abr): cache key builder collision — si el `cacheKeyBuilder` configurable produce la misma clave para tokens distintos, el verificador puede retornar los claims de un token incorrecto, causando **identity/authorization mixup**. Afecta versiones 0.0.1–6.1.x, fix en `6.2.0`. (2) **CVE-2026-35040** (CVSS 5.3, 9 abr): uso de RegExp con flag `/g` o `/y` en opciones de validación (`allowedAud`, `allowedIss`, etc.) produce validación no-determinista — 50% de requests válidos fallan alternadamente (logical DoS). Afecta `< 6.2.1`. (3) **CVE-2026-35041** (CVSS 4.2, 9 abr): ReDoS — RegExp con cuantificadores anidados (ej. `/(a+)+X$/`) en opciones `allowed*` permite a un atacante con token válido causar 100% CPU. Afecta versiones 5.0.0–6.2.0, fix en `6.2.1`.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `fast-jwt` a `>=6.2.1` para recibir todos los fixes. CVE-2026-35039 es el más crítico: en sistemas que hayan customizado `cacheKeyBuilder` con una función que produzca colisiones, usuarios pueden recibir claims de otros usuarios (escalada horizontal). CVEs 35040 y 35041 solo son explotables cuando se usan RegExp objects (no strings) en opciones de validación. También existe **CVE-2026-35042** (acepta extensiones `crit` desconocidas en violación de RFC 7515) — actualizar al mismo tiempo.

---

## [2026-04-10] CVE-2026-39865 - Axios HTTP/2: DoS por state corruption en session cleanup (CVSS 5.9)

- **Source**: [GitLab Advisory](https://advisories.gitlab.com/pkg/npm/axios/CVE-2026-39865/) / [CVEReports](https://cvereports.com/reports/CVE-2026-39865) / [Tenable](https://www.tenable.com/cve/CVE-2026-39865)
- **Confidence**: Alta
- **What changes**: CVE-2026-39865 (CVSS 5.9, MEDIUM, publicado 8 abril 2026). **Distinto del ataque a la supply chain de Axios (UNC1069)**. El método `Http2Sessions.getSession()` en `lib/adapters/http.js` itera hacia atrás un array mientras lo muta, causando state corruption cuando un servidor HTTP/2 malicioso cierra múltiples sesiones de forma concurrente. Causa crash del proceso Node.js del cliente. Solo afecta a requests que usan el adapter HTTP/2 (no el HTTP/1.1 default). Afecta `axios < 1.13.2`.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `axios` a `>=1.13.2`. El vector requiere que el atacante controle el servidor HTTP/2 al que el cliente Axios se conecta, o sea un MitM capaz de inyectar frames de control HTTP/2. No permite RCE ni acceso a datos — solo DoS. Riesgo real para microservicios que hagan requests entre sí con HTTP/2 en entornos donde un servicio comprometido pueda crashear clientes.

---

## [2026-04-10] "Tunnel Vision" - ataque a cadena de suministro via kube-health-tools (npm + PyPI), targeting Kubernetes

- **Source**: [Panther Security Blog](https://panther.com/blog/tunnel-vision-supply-chain-attack-targets-kubernetes-via-npm-and-pypi)
- **Confidence**: Alta
- **What changes**: Campaña activa desde el 1 de abril de 2026. El paquete `kube-health-tools` fue publicado en npm (5 versiones: 1.0.0, 1.0.1, 1.0.3, 2.0.0, 2.1.0) y PyPI (v1.0.9) desde la cuenta `hhsw2015`. El payload es una cadena de 4 etapas: (1) postinstall ejecuta un nativo addon/script, (2) decode XOR + ejecución de shell script, (3) descarga un binario Go desde github.com/gibunxi4201/kube-node-diag, (4) establece un **reverse tunnel TLS 1.3** a `sync[.]geeker[.]indevs[.]in:443` con acceso SOCKS5 + SSH, persistiendo como `node-health-check`. Objetivo: kubeconfig files, AWS keys, GCP service account JSON, Azure tokens y CI/CD secrets en workstations de desarrolladores.
- **Action required**: Urgente
- **Details**: IOCs: proceso `node-health-check` fuera de DaemonSets legítimos, archivo `/tmp/.nhc.enc`, C2 `sync[.]geeker[.]indevs[.]in`. Acciones: bloquear DNS del C2 en egress, buscar el proceso y archivo en todos los hosts de desarrollo y CI/CD, revisar `package-lock.json` y `pip freeze` por esas versiones, rotar kubeconfig + cloud creds + SSH keys + CI/CD secrets si se detecta compromiso. Los paquetes fueron removidos de npm y PyPI.

---

## [2026-04-10] UNC1069/Contagious Interview - campaña expandida a 1700+ paquetes en 5 ecosistemas (Go, Rust, PHP)

- **Source**: [The Hacker News (8 abr 2026)](https://thehackernews.com/2026/04/n-korean-hackers-spread-1700-malicious.html) / [Socket.dev Blog](https://socket.dev/blog/contagious-interview-campaign-spreads-across-5-ecosystems)
- **Confidence**: Alta
- **What changes**: Extiende las entradas de 2026-04-06 y 2026-04-08 sobre UNC1069/Sapphire Sleet/BlueNoroff. Reporte de Socket.dev del 8 de abril confirma que la misma campaña "Contagious Interview" lleva activa desde enero 2025 y ha publicado **1700+ paquetes maliciosos** en **5 ecosistemas**: npm, PyPI, **Go Modules** (nuevo), **crates.io/Rust** (nuevo) y **Packagist/PHP** (nuevo). Mismo actor UNC1069, misma infraestructura de staging. Paquetes identificados en este batch: npm: `dev-log-core`, `logger-base`, `logkitx`, `pino-debugger`, `debug-fmt`, `debug-glitz`; PyPI: `logutilkit`, `apachelicense`, `fluxhttp`, `license-utils-kit`. Payload: loaders que descargan RAT/infostealer de segunda etapa con keystroke logging, robo de browser data, despliegue de AnyDesk.
- **Action required**: Urgente
- **Details**: El alcance es mucho mayor de lo documentado previamente. Si el stack incluye Go, Rust o PHP, auditar también esas dependencias. Security Alliance (SEAL) bloqueó 164 dominios UNC1069 entre el 6 de febrero y 7 de abril de 2026. El código malicioso se inyecta en funciones legítimas (no en postinstall) para retrasar detección. Vector de ingeniería social: reclutadores falsos en LinkedIn/Telegram/Slack, entrevistas técnicas con "challenges" que incluyen los paquetes maliciosos.

---

## [2026-04-10] hermes-px - paquete PyPI malicioso: proxy AI falso que roba prompts con system prompt robado de Claude Code

- **Source**: [JFrog Security Research (5 abr 2026)](https://research.jfrog.com/post/hermes-px-pypi/) / [CybersecurityNews](https://cybersecuritynews.com/trojanized-pypi-ai-proxy-uses-stolen-claude-prompt/) / [SC Media](https://www.scworld.com/brief/malicious-pypi-package-enables-claude-prompt-data-compromise)
- **Confidence**: Alta
- **What changes**: JFrog identificó el 5 de abril de 2026 el paquete `hermes-px` en PyPI, que se presentaba como un "Secure AI Inference Proxy" que enrutaba requests via Tor. En realidad: (1) interceptaba todas las conversaciones del usuario y las exfiltraba a una base de datos Supabase controlada por el atacante (bypassing Tor, exponiendo la IP real), (2) redirigía los requests a un endpoint privado de universidad tunecina, (3) el archivo `base_prompt.pz` contenía un system prompt de **246.000 caracteres** que resultó ser una copia casi completa del system prompt interno de Claude Code de Anthropic — con marcadores internos que sugieren fue extraído de un deployment de finales de 2025 o principios de 2026. Las cadenas sensibles (credenciales Supabase, URLs) estaban protegidas con triple capa de cifrado para evadir scanners estáticos.
- **Action required**: Urgente
- **Details**: Verificar que `hermes-px` no esté instalado en ningún entorno. El paquete ya fue eliminado de PyPI. IOC: base de datos Supabase con modelos nombrados `OLYMPUS-1`, `AXIOM-1`, `hermes-v1`. Si el paquete fue instalado, rotar todos los API keys usados en prompts enviados durante el período de uso. La presencia de un system prompt completo de Claude Code en el paquete confirma que actores maliciosos tienen acceso a prompts internos no divulgados — posiblemente relacionado con la filtración de source maps de 2026-04-06.

---

## [2026-04-11] CVE-2026-40175 - Axios "Gadget Chain": Prototype Pollution → RCE / Full Cloud Compromise (CVSS 10.0)

- **Source**: [TheHackerWire CVE-2026-40175](https://www.thehackerwire.com/vulnerability/CVE-2026-40175/) / [Snyk axios](https://security.snyk.io/package/npm/axios) / [CVEDetails Axios](https://www.cvedetails.com/product/54129/Axios-Axios.html?vendor_id=19831)
- **Confidence**: Alta
- **What changes**: CVE-2026-40175 (CVSS 10.0, divulgado ~10 abril 2026) es una vulnerabilidad **distinta e independiente** del ataque a la supply chain (UNC1069, ya en archivo) y del DoS HTTP/2 (CVE-2026-39865, ya en archivo). En versiones `axios < 1.15.0`, el método `toJSON()` del objeto de configuración actúa como **gadget**: permite que cualquier Prototype Pollution en **cualquier dependencia del stack** (no necesariamente Axios) sea escalada a RCE completo o a Full Cloud Compromise via bypass de AWS IMDSv2. El atacante no necesita controlar directamente Axios — basta con explotar prototype pollution en cualquier otra librería del proyecto.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar `axios` a `>=1.15.0`. El fix elimina el gadget `toJSON()` que era el vector de escalación de la cadena. Especialmente crítico en entornos cloud donde AWS IMDSv2 es accesible desde el proceso Node.js — permite robo de credenciales IAM. **Nota**: si ya actualizaste a `1.13.x` para CVE-2026-39865 (DoS HTTP/2), debes actualizar **también** a `1.15.0` para este CVE.

---

## [2026-04-11] CVE-2026-39987 - Marimo Python Notebook: Pre-Auth RCE via WebSocket, explotación activa (CVSS 9.3)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/marimo-rce-flaw-cve-2026-39987.html) / [Endor Labs](https://www.endorlabs.com/learn/root-in-one-request-marimos-critical-pre-auth-rce-cve-2026-39987) / [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-39987) / [GitLab Advisory GHSA-2679-6MX9-H9XC](https://advisories.gitlab.com/pkg/pypi/marimo/GHSA-2679-6mx9-h9xc/) / [Vulert](https://vulert.com/blog/marimo-rce-flaw-cve-2026-39987-exploited/)
- **Confidence**: Alta
- **What changes**: CVE-2026-39987 (CVSS 9.3, publicado ~9-10 abril 2026) afecta Marimo, el notebook Python interactivo moderno alternativo a Jupyter. El endpoint WebSocket `/terminal/ws` omite completamente la validación de autenticación (a diferencia de otros endpoints WebSocket del mismo servidor que sí llaman `validate_auth()`), permitiendo que cualquier atacante de red obtenga un PTY shell completo con los privilegios del proceso Marimo — sin credenciales. Explotación activa confirmada: primer ataque en campo a los **9 horas y 41 minutos** tras la divulgación, registrado por Sysdig en honeypots. Añadido a CISA KEV; deadline CISA para agencias federales: **11 de abril de 2026**. Afecta todas las versiones `marimo <= 0.20.4`.
- **Action required**: Urgente
- **Details**: Actualizar `marimo` a `>=0.23.0`. Si Marimo está expuesto en red (sin VPN, servidor compartido, Jupyter Hub), tratar como potencialmente comprometido hasta aplicar el parche. La vulnerabilidad requiere solo un cliente WebSocket estándar — ningún otro prerrequisito. GHSA: `GHSA-2679-6MX9-H9XC`.

---

## [2026-04-11] GitHub Copilot VS Code (marzo 2026) - Autopilot autónomo, subagentes anidados, video/imagen en chat

- **Source**: [GitHub Changelog - GitHub Copilot in Visual Studio Code, March Releases (8 abr 2026)](https://github.blog/changelog/2026-04-08-github-copilot-in-visual-studio-code-march-releases/)
- **Confidence**: Alta
- **What changes**: El changelog oficial de GitHub del 8 de abril 2026 (distinto de los changelogs ya documentados sobre SDK, privacidad, CLI BYOK y cloud agent) publica los releases de VS Code de marzo. Cuatro adiciones no cubiertas en entradas anteriores: (1) **Autopilot** (public preview) — los agentes aprueban sus propias acciones, reintentan errores automáticamente y completan tareas sin intervención manual (`chat.autopilot.enabled`). (2) **Subagentes anidados** (`chat.subagents.allowInvocationsFromSubagents`) — subagentes pueden invocar otros subagentes para workflows multi-paso complejos. (3) **Imágenes y video en chat** — adjuntar screenshots/videos a mensajes; los agentes retornan grabaciones/imágenes de los cambios con visor tipo carrusel con zoom/pan. (4) **Editor unificado de customizaciones de chat** — instrucciones, agentes custom, skills y plugins desde un único panel; browsing del marketplace MCP integrado.
- **Action required**: Monitorear
- **Details**: Autopilot es el cambio más relevante: elimina la fricción de aprobación manual en sesiones largas, habilitando tareas completamente desatendidas en VS Code. Los subagentes anidados son el complemento VS Code de las Agents Window de Cursor 3.0 y los Managed Agents de Anthropic — indica convergencia del ecosistema hacia multi-agente. Esta entrada complementa las entradas de Copilot del 2026-04-07 y 2026-04-08.

---

## [2026-04-11] Microsoft MAI-Transcribe-1, MAI-Voice-1, MAI-Image-2 - modelos propios en Foundry (2 abril 2026)

- **Source**: [Microsoft AI oficial](https://microsoft.ai/news/today-were-announcing-3-new-world-class-mai-models-available-in-foundry/) / [TechCrunch](https://techcrunch.com/2026/04/02/microsoft-takes-on-ai-rivals-with-three-new-foundational-models/) / [VentureBeat](https://venturebeat.com/technology/microsoft-launches-3-new-ai-models-in-direct-shot-at-openai-and-google) / [Microsoft Tech Community](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/introducing-mai-transcribe-1-mai-voice-1-and-mai-image-2-in-microsoft-foundry/4507787)
- **Confidence**: Alta
- **What changes**: Microsoft lanzó el 2 de abril de 2026 tres modelos de producción desarrollados internamente (no de OpenAI/Anthropic), disponibles en Microsoft Foundry y el nuevo MAI Playground (solo US por ahora): (1) **MAI-Transcribe-1** — STT para 25 idiomas a ~50% menor costo GPU que competidores; 2.5x más rápido en batch transcription vs. el Azure Fast offering. Compite directamente con Whisper y Cohere Transcribe. (2) **MAI-Voice-1** — TTS que genera 60 segundos de audio en <1 segundo en un solo GPU; voice cloning con pocos segundos de audio. Compite con Voxtral de Mistral. (3) **MAI-Image-2** — generación de imagen, #3 en Arena.ai leaderboard, 2x más rápido que la generación previa de Microsoft. Precios: $0.36/hr (Transcribe), $22/M chars (Voice), $5/M tokens input + $33/M tokens image output (Image-2).
- **Action required**: Monitorear
- **Details**: Primera señal concreta de que Microsoft reduce dependencia de OpenAI desarrollando modelos propios de producción. Para equipos en el ecosistema Azure/Microsoft Foundry: estos modelos eliminan la necesidad de APIs de terceros para STT/TTS/imagen. MAI-Transcribe-1 es relevante como alternativa a Whisper con menor costo GPU; compite con Cohere Transcribe (Apache 2.0, ya en el archivo) aunque MAI no es open-weight.

---

## [2026-04-11] Claude Code v2.1.101 - fix de command injection, /team-onboarding, OS CA cert trust por defecto

- **Source**: [claudeupdates.dev/version/2.1.101](https://www.claudeupdates.dev/version/2.1.101) / [GitHub Releases anthropics/claude-code](https://github.com/anthropics/claude-code/releases)
- **Confidence**: Alta
- **What changes**: Claude Code v2.1.101 (10 abril 2026, 46 cambios, 65% bug fixes) supercede la entrada de v2.1.98. Fix de seguridad crítico: **command injection en el fallback POSIX `which`** — el código que buscaba ejecutables en el PATH podía ser explotado vía nombres de binarios maliciosos. Adiciones: (1) nuevo comando `/team-onboarding` que genera guías de ramp-up para compañeros de equipo desde el historial de uso local; (2) el OS CA certificate store se confía por defecto para proxies TLS empresariales (elimina configuración manual para entornos corporativos con interceptación TLS). Mejoras: Focus mode produce summaries más autocontenidos, mensajes de rate-limit ahora muestran cuál límite se activó y el tiempo de reset, resumption acepta títulos seteados via `/rename`. Fixes de crashes (3 escenarios distintos), memory leak del virtual scroller, pérdida de contexto en `/resume` en conversaciones largas, y 6 bugs adicionales de `/resume`.
- **Action required**: Monitorear
- **Details**: El fix de command injection es el item de seguridad más importante — actualizar a v2.1.101 si se usa Claude Code con binarios en paths no estándar. El OS CA trust por defecto es un cambio de comportamiento que puede resolver problemas en entornos con SSL inspection/MITM proxies corporativos sin config extra. Supercede las entradas previas de v2.1.97 y v2.1.98.

---

## [2026-04-11] CVE-2026-1340 (Ivanti EPMM) - CISA deadline hoy: RCE sin autenticación en MDM, CVSS 9.8

- **Source**: [Unit 42 / Palo Alto](https://unit42.paloaltonetworks.com/ivanti-cve-2026-1281-cve-2026-1340/) / [Tenable](https://www.tenable.com/blog/cve-2026-1281-cve-2026-1340-ivanti-endpoint-manager-mobile-epmm-zero-day-vulnerabilities) / [CyCognito](https://www.cycognito.com/blog/emerging-threat-cve-2026-1281-cve-2026-1340-ivanti-epmm-unauthenticated-rce-via-code-injection/) / [Security Affairs - CISA KEV](https://securityaffairs.com/190519/security/u-s-cisa-adds-a-flaw-in-ivanti-epmm-to-its-known-exploited-vulnerabilities-catalog-2.html)
- **Confidence**: Alta
- **What changes**: CVE-2026-1340 (CVSS 9.8, CRÍTICO) fue añadido al CISA KEV catalog el 9-10 de abril de 2026, con **deadline de mitigación para agencias federales: 11 de abril de 2026 (hoy)**. Complementa CVE-2026-1281 (añadido a KEV en enero). Ambos afectan Ivanti Endpoint Manager Mobile (EPMM) on-premises ≤12.7.x. El vector es HTTP GET a endpoints `/mifs/c/aftstore/fob/` — unsafe bash `$()` arithmetic expansion sin sanitización de input permite command injection → RCE sin autenticación ni interacción de usuario. Unit 42 observó explotación masiva y automatizada. Afecta infraestructura MDM que gestiona dispositivos móviles corporativos (configuraciones, VPN, email, apps).
- **Action required**: Urgente
- **Details**: Aplicar RPM 12.x.0.x o RPM 12.x.1.x según la versión instalada. Solo afecta instalaciones Ivanti EPMM on-premises — Ivanti Cloud no está afectado. En entornos de desarrollo que usen MDM corporativo gestionado con EPMM: rotar credenciales corporativas, VPN y email si el servidor EPMM no está parcheado, ya que el servidor puede haber sido comprometido y tiene acceso a todos los dispositivos gestionados.

---

## [2026-04-11] CVE-2026-39363 - Vite Dev Server: arbitrary file read via WebSocket fetchModule (CVSS 8.2)

- **Source**: [GitHub Advisory GHSA-p9ff-h696-f583](https://github.com/vitejs/vite/security/advisories/GHSA-p9ff-h696-f583) / [CIRCL Vulnerability Lookup](https://vulnerability.circl.lu/vuln/ghsa-p9ff-h696-f583)
- **Confidence**: Alta
- **What changes**: CVE-2026-39363 (CVSS 8.2, HIGH, CWE-22) publicado el 7 de abril de 2026. El método `fetchModule` expuesto via el WebSocket del Vite dev server (`vite:invoke` custom event) no aplica las restricciones del `server.fs` config. Un atacante que pueda conectarse al WebSocket sin header `Origin` puede usar `file://...?raw` o `file://...?inline` para leer archivos arbitrarios del host del servidor. Vector de red, sin autenticación. Afecta Vite 6.0.0–6.4.1, 7.0.0–7.3.1, 8.0.0–8.0.4. PoC disponible; sin explotación activa confirmada.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a Vite **6.4.2**, **7.3.2** o **8.0.5** según la rama. La vulnerabilidad solo afecta cuando el dev server está **expuesto a la red** (`--host` flag o `server.host` en config) y el WebSocket no está desactivado (`server.ws: false`). Entornos de desarrollo locales (localhost only) no están afectados. Relevante para: servidores de dev compartidos, CI/CD pipelines que ejecuten `vite dev` con `--host`, Docker containers con Vite expuesto en red interna. GHSA: `GHSA-p9ff-h696-f583`.

---

## [2026-04-11] CVE-2026-5724 - Temporal Server: autenticación ausente en streaming gRPC AdminService (CVSS 6.3)

- **Source**: [CIRCL Vulnerability Lookup](https://vulnerability.circl.lu/vuln/cve-2026-5724) / [GitHub Advisory GHSA-q98v-9f9w-f49q](https://github.com/temporalio/temporal/security)
- **Confidence**: Alta
- **What changes**: CVE-2026-5724 (CVSS 6.3, MEDIUM, CWE-306) publicado el 10 de abril de 2026. El interceptor chain del servidor gRPC frontend de Temporal omite el interceptor de autorización para endpoints de streaming. El endpoint `AdminService/StreamWorkflowReplicationMessages` acepta requests sin credenciales, a diferencia de los endpoints unary que sí aplican autenticación. Un atacante con acceso de red al puerto frontend de Temporal y conocimiento de la configuración del cluster puede exfiltrar datos de replicación de workflows. Solo afecta Temporal Server self-hosted (Temporal Cloud no está afectado). Afecta versiones 1.24.0–1.30.3.
- **Action required**: Monitorear
- **Details**: Actualizar Temporal Server a **v1.28.4**, **v1.29.6**, o **v1.30.4** según la rama. El riesgo real requiere: (1) acceso de red al puerto gRPC frontend (7233 por defecto) desde redes no confiables, y (2) conocimiento del cluster ID y peers. Equipos que expongan el puerto 7233 públicamente o en redes internas amplias sin mTLS/firewall deben priorizar el upgrade. Temporal Cloud no está afectado. GHSA: `GHSA-q98v-9f9w-f49q`.

---

## [2026-04-12] Chrome 146 DBSC - Device Bound Session Credentials GA en Windows (anti-cookie theft)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/google-rolls-out-dbsc-in-chrome-146-to.html) / [Help Net Security](https://www.helpnetsecurity.com/2026/04/10/google-chrome-device-bound-session-credentials/) / [gHacks](https://www.ghacks.net/2026/04/10/google-chrome-146-adds-device-bound-session-credentials-to-stop-session-cookie-theft-on-windows/)
- **Confidence**: Alta
- **What changes**: Google activó Device Bound Session Credentials (DBSC) en Chrome 146 para todos los usuarios Windows el 9 de abril de 2026. DBSC vincula criptográficamente las session cookies al chip TPM del dispositivo usando un key pair único por sitio — cualquier cookie robada por malware es inútil en otro dispositivo. Requiere TPM 2.0 o Windows Hello-compatible hardware. Disponible en Windows 10 v1903+ y Windows 11; cubre ~85% de instalaciones Windows Chrome según telemetría de Google. Expansión a macOS planeada en versión futura de Chrome.
- **Action required**: Monitorear
- **Details**: No requiere acción obligatoria para desarrolladores web — DBSC tiene fallback graceful en dispositivos sin TPM. Los sitios pueden optar por soporte activo de DBSC para sesiones de alta seguridad (verificación explícita de key possession). Impacto principal: los ataques de session hijacking via infostealer malware (vector usado en supply chain attacks como UNC1069 y Axios compromise) pierden efectividad para usuarios con TPM 2.0. Google reporta "reducción significativa de session theft desde el beta". Para equipos de seguridad web: evaluar implementar soporte activo DBSC en aplicaciones críticas una vez que la API esté ampliamente disponible.

---

## [2026-04-12] CVE-2026-35616 - Fortinet FortiClient EMS: RCE sin autenticación, explotación activa (CVSS 9.1)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/fortinet-patches-actively-exploited-cve.html) / [BleepingComputer](https://www.bleepingcomputer.com/news/security/new-fortinet-forticlient-ems-flaw-cve-2026-35616-exploited-in-attacks/) / [CISA KEV oficial](https://www.cisa.gov/news-events/alerts/2026/04/06/cisa-adds-one-known-exploited-vulnerability-catalog) / [watchTowr](https://watchtowr.com/resources/fortinet-forticlient-ems-zero-day-cve-2026-35616-active-exploitation-underway/)
- **Confidence**: Alta
- **What changes**: CVE-2026-35616 (CVSS 9.1, CWE-284) en Fortinet FortiClient EMS (Enterprise Management Server) versiones 7.4.5–7.4.6. Un atacante no autenticado puede enviar requests crafteados que eluden los controles de acceso de la API y ejecutar código arbitrario en el servidor EMS. Afecta **únicamente el servidor EMS** — no el cliente desktop FortiClient. Explotación activa confirmada desde el 31 de marzo de 2026; CISA lo añadió al KEV catalog el 6 de abril con deadline para agencias federales el 9 de abril de 2026 (ya vencido).
- **Action required**: Urgente
- **Details**: Aplicar hotfix disponible para versiones 7.4.5 y 7.4.6; fix completo en FortiClient EMS 7.4.7 (pendiente). Si el servidor EMS está expuesto a redes no confiables sin firewall, tratar como potencialmente comprometido y hacer forensics antes de parchear. FortiClient EMS gestiona políticas de seguridad endpoint para toda la organización — compromiso del servidor expone configuración de VPN, compliance policies y credenciales de todos los endpoints gestionados. Relevante en entornos enterprise con Fortinet como proveedor de seguridad endpoint.

---

## [2026-04-12] CVE-2026-33017 - Langflow: RCE no autenticado (CVSS 9.8), CISA KEV, explotación en 20 horas

- **Source**: [The Hacker News](https://thehackernews.com/2026/03/critical-langflow-flaw-cve-2026-33017.html) / [Sysdig](https://www.sysdig.com/blog/cve-2026-33017-how-attackers-compromised-langflow-ai-pipelines-in-20-hours) / [JFrog Research](https://research.jfrog.com/post/langflow-latest-version-was-not-fixed/) / [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) / [Help Net Security](https://www.helpnetsecurity.com/2026/03/27/cve-2026-33017-cve-2026-33634-exploited/)
- **Confidence**: Alta
- **What changes**: CVE-2026-33017 (CVSS 9.8, divulgado el 17 de marzo de 2026) es un RCE no autenticado en el endpoint `POST /api/v1/build_public_tmp/{flow_id}/flow` de Langflow. El servidor acepta definiciones de flujo controladas por el atacante que contienen código Python arbitrario en nodos, y lo ejecuta sin sandbox ni autenticación. Explotación activa confirmada a las **20 horas de la divulgación** (Sysdig honeypot). CISA añadió al KEV con deadline para agencias federales el **8 de abril de 2026** (ya vencido). JFrog descubrió que la versión 1.8.2 (inicialmente reportada como parcheada) **sigue siendo explotable** — el fix real es 1.9.0.
- **Action required**: Urgente
- **Details**: Actualizar Langflow a `>=1.9.0` (NO 1.8.2 — sigue vulnerable según JFrog). Las instancias Langflow típicamente contienen API keys de OpenAI, Anthropic, Azure OpenAI, AWS Bedrock y Google Vertex configuradas como credentials — tratar cualquier instancia que haya corrido 1.8.2 como potencialmente comprometida y rotar todos los secrets. Sin acceso de red al endpoint `/api/v1/build_public_tmp/` no hay remediación temporal viable. Instancias expuestas en internet: ~12,000 según Shodan.

---

## [2026-04-12] CVE-2026-34070 + CVE-2025-68664 + CVE-2025-67644 - LangChain/LangGraph: path traversal, deserialization y SQL injection (divulgados 27 mar 2026)

- **Source**: [The Hacker News](https://thehackernews.com/2026/03/langchain-langgraph-flaws-expose-files.html) / [Cyata "LangGrinch"](https://cyata.ai/blog/langgrinch-langchain-core-cve-2025-68664/) / [GitHub Advisory GHSA](https://github.com/advisories)
- **Confidence**: Alta
- **What changes**: El 27 de marzo de 2026, Cyera divulgó tres vulnerabilidades en los frameworks más usados para LLM applications (60M descargas semanales en PyPI): (1) **CVE-2026-34070** (CVSS 7.5) — path traversal en `langchain_core/prompts/loading.py`: plantillas de prompt maliciosas permiten leer archivos arbitrarios del filesystem sin validación. Fix: `langchain-core >=1.2.22`. (2) **CVE-2025-68664** "LangGrinch" (CVSS 9.3) — deserialización de datos no confiables: una estructura crafteada enviada como input engaña a LangChain para interpretarla como objeto LangChain ya serializado, filtrando API keys y secrets del entorno. Fix: `langchain-core 0.3.81` (rama 0.3.x) o `1.2.5` (rama 1.2.x). (3) **CVE-2025-67644** (CVSS 7.3) — SQL injection en `LangGraph` checkpoint SQLite: los metadata filter keys permiten manipular queries SQL arbitrarias contra la DB. Fix: `langgraph-checkpoint-sqlite >=3.0.1`.
- **Action required**: Actualizar dependencia
- **Details**: El CVE-2025-68664 es el más crítico (CVSS 9.3): cualquier aplicación LangChain que procese input no confiable del usuario puede exponer `OPENAI_API_KEY`, `ANTHROPIC_API_KEY` y otras variables de entorno. El path traversal (CVE-2026-34070) afecta a cualquier app que use `load_prompt()` con inputs del usuario. Ejecutar `pip show langchain-core langgraph-checkpoint-sqlite` para verificar versiones. Para proyectos que usen LangGraph con persistencia: también actualizar `langgraph-checkpoint-sqlite`.

---

## [2026-04-12] Veeam Backup & Replication - 7 CVEs críticos/altos (CVSS hasta 9.9), RCE autenticado, parches marzo 2026

- **Source**: [The Hacker News](https://thehackernews.com/2026/03/veeam-patches-7-critical-backup.html) / [Veeam KB4792](https://www.veeam.com/kb4792) / [Veeam KB4830](https://www.veeam.com/kb4830) / [BleepingComputer](https://www.bleepingcomputer.com/news/security/veeam-warns-of-critical-flaws-exposing-backup-servers-to-rce-attacks/)
- **Confidence**: Alta
- **What changes**: El 13 de marzo de 2026, Veeam publicó parches para 7 vulnerabilidades en Backup & Replication v12.x y v13.x. Las más críticas: **CVE-2026-21666, -21667, -21669** (CVSS 9.9 cada una) — usuarios de dominio autenticados con privilegios bajos pueden ejecutar código remoto en el servidor Veeam Backup vía métodos RPC sin restricciones. **CVE-2026-21708** (CVSS 9.9) — Backup Viewer puede ejecutar código arbitrario como usuario `postgres`. **CVE-2026-21671** (CVSS 9.1) — RCE en deployments de alta disponibilidad. **CVE-2026-21668** y **-21672** (CVSS 8.8) — file manipulation y escalación de privilegios local.
- **Action required**: Actualizar dependencia
- **Details**: Versiones parcheadas: **v12.3.2.4465** (para rama 12.x) y **v13.0.1.2067** (para rama 13.x). Sin explotación activa confirmada al momento de la divulgación, pero Veeam es objetivo histórico de grupos de ransomware (los CVEs de Veeam son invariablemente weaponizados rápidamente tras disclosure). Infraestructura de backup es objetivo de alta prioridad porque: (1) contiene credenciales de todos los sistemas gestionados, (2) los attackers pueden sabotear backups antes de lanzar ransomware. Priorizar el patch especialmente si el puerto Veeam está accesible desde la red corporativa.

---

## [2026-04-12] DarkSword - kit de exploit iOS full-chain, parche Apple iOS 18.7.7 (1-2 abril 2026)

- **Source**: [The Hacker News](https://thehackernews.com/2026/04/apple-expands-ios-1877-update-to-more.html) / [Google Cloud GTIG](https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain) / [iVerify](https://iverify.io/blog/darksword-ios-exploit-kit-explained) / [Malwarebytes](https://www.malwarebytes.com/blog/mobile/2026/03/a-darksword-hangs-over-unpatched-iphones) / [Help Net Security](https://www.helpnetsecurity.com/2026/03/19/darksword-ios-exploit-iphone/)
- **Confidence**: Alta
- **What changes**: DarkSword es un exploit kit iOS full-chain que encadena **6 vulnerabilidades** en WebKit, Safari, el dynamic loader y el kernel para escalar de "usuario visita sitio web" a compromiso completo del dispositivo (sin interacción adicional). Afecta iOS/iPadOS **18.4 a 18.7** exclusivamente. Vector: ataque de watering hole — el usuario visita un sitio web legítimo pero comprometido. Payload: backdoor persistente + data miner. En uso activo desde julio 2025 por actores estatales: PARS Defense (empresa de surveillance turca), UNC6748, y UNC6353 (grupo de espionaje vinculado a Rusia). Objetivos identificados: usuarios en Arabia Saudita, Turquía, Malasia y Ucrania. Apple lanzó **iOS/iPadOS 18.7.7** el 1-2 de abril de 2026, un patch inusual dentro de la misma rama de OS.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar a iOS/iPadOS 18.7.7 inmediatamente. El parche fue expandido a dispositivos adicionales (iPhone XR en adelante, iPad mini 5a gen+, iPad Air 3a-5a gen, iPad Pro varios modelos) el 2 de abril. Relevante para equipos de desarrollo mobile: (1) verificar que los builds internos y CI use iOS 18.7.7 como minimum deployment target para testing si se manejan datos sensibles, (2) comunicar a usuarios finales la urgencia del update especialmente en organizaciones con usuarios en regiones objetivo. Apple califica el parche como "Rapid Security Response" — activar actualizaciones automáticas en dispositivos gestionados por MDM.

---

## [2026-04-13] Windows Secure Boot 2011 expira junio 2026 — Windows Server requiere acción manual (sin actualización automática)

- **Source**: [Windows Server Secure Boot Playbook oficial (Microsoft Tech Community)](https://techcommunity.microsoft.com/blog/windowsservernewsandbestpractices/windows-server-secure-boot-playbook-for-certificates-expiring-in-2026/4495789) / [Help Net Security (3 abr 2026)](https://www.helpnetsecurity.com/2026/04/03/windows-secure-boot-certificate-update-2026-expiration/) / [Microsoft Windows Server Blog (23 feb 2026)](https://www.microsoft.com/en-us/windows-server/blog/2026/02/23/prepare-your-servers-for-secure-boot-certificate-updates/) / [Windows Latest (3 abr 2026)](https://www.windowslatest.com/2026/04/03/windows-11-will-now-tell-you-if-your-secure-boot-certificates-need-attention/)
- **Confidence**: Alta
- **What changes**: Los certificados Secure Boot de Microsoft emitidos en 2011 expiran en 2026: **Microsoft Corporation KEK CA 2011** y **Microsoft Corporation UEFI CA 2011** expiran **junio 2026** (~73 días); **Microsoft Windows Production PCA 2011** (firma el bootloader de Windows) expira octubre 2026. Diferencia crítica: los PCs Windows reciben los nuevos certificados 2023 automáticamente vía Windows Update/CFR. Los **Windows Servers NO los reciben automáticamente** — requieren acción manual del administrador. Desde el 8 de abril 2026, la Windows Security App muestra el estado de los certificados Secure Boot bajo "Device security > Secure Boot" (activo en Windows 11 23H2-26H1 y Windows Server 2025 desde el 8 abril; en Windows 10 22H2/21H2/1809 y Windows Server 2019/2022 Desktop, disponible vía cumulative update del 14 abril 2026).
- **Action required**: Urgente
- **Details**: Para entornos con Windows Servers: (1) Verificar estado: `Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecureBoot" -Name UEFICA2023Status` — valor `1` = certificados 2023 presentes. (2) Si no los tiene, aplicar manualmente siguiendo el playbook del Tech Community. Tras la expiración en junio 2026, los servidores sin los certificados 2023 dejan de recibir actualizaciones de seguridad del proceso de arranque temprano y no confían en software de terceros firmado con los nuevos certs (UEFI drivers, bootloaders de terceros, dual-boot Linux, VMware/Oracle). VMs también afectadas si el hypervisor host no tiene los certs 2023. Los fabricantes de hardware (Dell, HP, Lenovo) que enviaron servidores con certificados 2023 preinstalados no requieren acción adicional — verificar con el vendor.

---

## [2026-04-13] CVE-2026-34980 + CVE-2026-34990 - OpenPrinting CUPS: cadena RCE no autenticado + root file overwrite en Linux (sin parche oficial aún)

- **Source**: [The Register (6 abr 2026)](https://www.theregister.com/2026/04/06/ai_agents_cups_server_rce/) / [pbxscience.com](https://pbxscience.com/critical-flaw-chain-in-linux-printing-system-cups-enables-remote-root-access/) / [OffSeq CVE-2026-34990](https://radar.offseq.com/threat/cve-2026-34990-cwe-287-improper-authentication-in--2f45c293) / [OffSeq CVE-2026-34980](https://radar.offseq.com/threat/cve-2026-34980-cwe-20-improper-input-validation-in-692dc2f6)
- **Confidence**: Alta
- **What changes**: Divulgados el 3-6 de abril de 2026. Dos CVEs que se encadenan para dar RCE no autenticado + sobreescritura arbitraria de archivos como root en sistemas Linux con CUPS: (1) **CVE-2026-34980** (CWE-20) — entrada no validada en cupsd de una cola PostScript compartida: un cliente no autenticado envía un Print-Job, inyecta newlines en el campo `page-border` que CUPS reinterpreta como directiva PPD de confianza, y ejecuta un binario existente (ej. `/usr/bin/vim`) con privilegios `lp`. (2) **CVE-2026-34990** (CWE-287) — token de autenticación local reutilizable: un usuario local sin privilegios fuerza a cupsd a autenticarse contra un servicio IPP atacante en localhost, obtiene el token `Authorization: Local` y lo usa para crear una cola con URI `file:///` que permite sobreescribir archivos como root. Encadenados: atacante remoto sin autenticación → RCE + root file overwrite. CVEs adicionales en el mismo lote: CVE-2026-34978/34979/34990, CVE-2026-27447. Descubiertos por un equipo de agentes AI dirigidos por Asim Viladi Oglu Manizada (SpaceX Security). Afecta CUPS ≤ 2.4.16.
- **Action required**: Monitorear
- **Details**: **Sin release oficial parcheado al 13 de abril de 2026** — solo commits en el repositorio público de OpenPrinting/cups. Mitigaciones temporales: (1) deshabilitar `printer-is-shared` en `/etc/cups/cupsd.conf` si no se necesita compartir impresoras en red, (2) restringir acceso de red al puerto 631 (IPP) con firewall, (3) deshabilitar cuentas locales no confiables. Verificar estado del parche en https://github.com/OpenPrinting/cups/releases. Relevante para: servidores Linux en red, CI/CD runners Linux con CUPS instalado, Docker hosts con CUPS. El attack vector requiere que la impresora esté configurada como compartida (`printer-is-shared=true`) — configuración por defecto en algunos sistemas Linux.

---

## [2026-04-13] CVE-2026-34743 - XZ Utils: buffer overflow en lzma_index_append() (CVSS 9.8), parcheado en 5.8.3

- **Source**: [tukaani.org oficial](https://tukaani.org/xz/index-append-overflow.html) / [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2026-34743/) / [Snyk](https://security.snyk.io/vuln/SNYK-DEBIAN11-XZUTILS-15896776) / [Red Hat Bugzilla](https://bugzilla.redhat.com/show_bug.cgi?id=2454589) / [BeyondMachines](https://beyondmachines.net/event_details/xz-utils-5-8-3-released-to-patch-buffer-overflow-and-memory-access-flaws-8-i-t-m-h/gD2P6Ple2L)
- **Confidence**: Alta
- **What changes**: CVE-2026-34743 (CVSS 9.8) publicado el 3 de abril de 2026. En XZ Utils 5.0.0–5.8.2, si `lzma_index_decoder()` procesa un Index que no contiene Records, el `lzma_index` resultante queda en estado que hace que un `lzma_index_append()` posterior asigne memoria insuficiente, causando heap buffer overflow que puede llevar a RCE o crash. Fix: `lzma_index_append()` ahora usa `INDEX_GROUP_SIZE` por defecto cuando `records == 0`. Parcheado en **XZ Utils 5.8.3** (lanzado el 3 de abril de 2026). CVE adicional en el mismo release: memory access flaw en herramienta xz CLI en sistemas 32-bit con nombres de archivo >2 GiB.
- **Action required**: Actualizar dependencia
- **Details**: Actualizar XZ Utils a `>=5.8.3`. Las distribuciones Linux (Debian, Ubuntu, Fedora, RHEL, SUSE, Arch) han emitido actualizaciones de paquete. A pesar del CVSS 9.8, el riesgo real es bajo: las funciones `lzma_index_*` son raramente usadas directamente por aplicaciones, y la combinación específica (decode de index vacío → append) es infrecuente en producción. El BSI alemán y otras agencias asignaron severity crítica. Relevante para proyectos que usen liblzma directamente (no la mayoría de apps — la CLI de xz y el formato `.xz` para compresión no están afectados en uso normal). Sin explotación activa conocida.
