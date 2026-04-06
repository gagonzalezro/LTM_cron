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
