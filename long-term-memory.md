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
