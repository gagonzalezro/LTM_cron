## [2026-05-05] Node.js 26 Released - Temporal API Enabled by Default

- **Fuente**: [Help Net Security](https://www.helpnetsecurity.com/2026/05/07/node-js-26-released/) / [Node.js Official](https://nodejs.org/en/blog/release/v26.0.0/)
- **Confianza**: Alta
- **Que cambia**: Node.js 26 (lanzado May 5, 2026) habilita la Temporal Date/Time API por defecto en lugar de experimental. Upgrades V8 a version 14.6. Removal de APIs deprecadas: `--experimental-transform-types` flag removed.
- **Accion requerida**: Monitorear
- **Detalles**: La Temporal API es un reemplazo moderno para `Date` que resuelve muchos problemas de timezone y precision. Es backward-compatible pero los desarrolladores pueden optar por usarlo. El removal de `--experimental-transform-types` afecta a proyectos que strippean tipos TypeScript en runtime. Verificar si tu proyecto usa esta flag.

---

## [2026-05-06] React y Next.js Security Vulnerabilities - 12 CVEs Patched

- **Fuente**: [Vercel Changelog](https://vercel.com/changelog/next-js-may-2026-security-release) / [Netlify Changelog](https://www.netlify.com/changelog/2026-05-08-react-nextjs-security-vulnerabilities/) / [Cloudflare](https://developers.cloudflare.com/changelog/post/2026-05-06-react-nextjs-vulnerabilities/)
- **Confianza**: Alta
- **Que cambia**: 12 vulnerabilities en React/Next.js: (1) 1 vulnerability en React Server Components (DoS), (11) vulnerabilities en Next.js (middleware bypass, XSS, SSRF, cache poisoning, DoS).
- **Accion requerida**: Urgente
- **Detalles**: CVE-2026-23870 (DoS) - atacantes pueden crashear servidores de produccion enviando requests especialmente crafted a Server Function endpoints. Patched: Next.js 15.5.18, 16.2.6; react-server-dom 19.0.6 / 19.1.7 / 19.2.6. La mitad de las vulnerabilities afectan apps que usan Server Components. Actualizar inmediatamente.

---

## [2026-05-19] Python 3.9 EOL on Dependabot - June 23, 2026

- **Fuente**: [GitHub Changelog](https://github.blog/changelog/2026-05-19-upcoming-deprecation-of-python-3-9-for-dependabot/)
- **Confianza**: Alta
- **Que cambia**: GitHub Dependabot dejara de soportar Python 3.9 el 23 de junio, 2026. Python 3.9 ya alcanzo su End-of-Life en octubre 31, 2025. Despues de junio, Dependabot no procesara updates para proyectos que specifique `python_version == "3.9"`.
- **Accion requerida**: Actualizar dependencia
- **Detalles**: Si tu proyecto aun usa Python 3.9, necesitas migrar a Python 3.10+ antes de junio 23. Dependabot silenciosamente dejara de procesar security updates para 3.9 projects sin advertencia en ese momento. La mayoria de librerias ya droppeo soporte para 3.9. Herramientas como `pyupgrade --py310-plus` pueden automatizar migraciones menores.

---

## [2026-05-15] Node.js 26 - Type Stripping and TypeScript Integration Changes

- **Fuente**: [Node.js 26 Release Notes](https://nodejs.org/en/blog/release/v26.0.0/) / [Hacker News Discussion](https://news.ycombinator.com/item?id=48023795)
- **Confianza**: Media
- **Que cambia**: Node.js 26 expands TypeScript/JSX support. El flag `--experimental-transform-types` (que strippea tipos TypeScript en runtime) ha sido removido de Node.js 26. El soporte para stripping de tipos se gestiona ahora via other mechanisms.
- **Accion requerida**: Monitorear
- **Detalles**: Si usabas `--experimental-transform-types` para ejecutar TypeScript directamente en Node.js sin compilacion previa, necesitas migrar a esbuild, tsx, o TypeScript compiler. El soporte para .ts files directamente sin flags aun no es stable.

---

## [2026-05-08] Next.js 15 Caching Semantics Breaking Change

- **Fuente**: [Vercel Docs - Upgrading to Version 16](https://nextjs.org/docs/app/guides/upgrading/version-16) / [Next.js Blog](https://nextjs.org/blog/)
- **Confianza**: Alta
- **Que cambia**: Fetch requests, GET Route Handlers, y navegaciones client no son cacheadas por defecto en Next.js 15 (cambia del comportamiento anterior).
- **Accion requerida**: Monitorear
- **Detalles**: Este es un cambio silencioso que puede afectar performance de apps existentes. Si dependias del caching implicito, necesitas habilitar `cache: 'force-cache'` explicitamente en tus fetch calls. La razon del cambio es simplificar el modelo de caching que antes tenia comportamiento no intuitivo.

---

## [2026-05-12] Deno 2.8 Release - setTimeout returns NodeJS.Timeout

- **Fuente**: [Deno Blog](https://deno.com/blog/v2.8) / [Deno Release Notes](https://github.com/denoland/deno/releases)
- **Confianza**: Media
- **Que cambia**: En Deno 2.8 (mayo 2026), `setTimeout()` ahora retorna un objeto `NodeJS.Timeout` en lugar de un numero. Esto causa breaking changes en codigo que asume return type es numero.
- **Accion requerida**: Actualizar dependencia
- **Detalles**: El cambio es para mejorar compatibilidad con Node.js pero rompe codigo que usa `typeof timerId === 'number'` checks. Solucion: usar `clearTimeout()` y `clearInterval()` que funcionan con ambos tipos. Este cambio es parte de la migracion de Deno hacia mayor compatibilidad con Node.js.
