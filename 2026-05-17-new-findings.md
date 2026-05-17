# Investigador - Hallazgos Validados [2026-05-17]

## [2026-05-05] Node.js 26 released - V8 14.6, Temporal API habilitado por defecto

- **Fuente**: [nodejs.org releases](https://nodejs.org/en/blog/announcements/nodejs-26-release)
- **Confianza**: Alta
- **Que cambia**: Node.js 26 lanzado como Current release (no LTS). Cambios principales: (1) V8 JavaScript engine actualizado a 14.6 — mejoras significativas de performance y nuevas características ECMAScript. (2) Temporal date/time API habilitado por defecto — reemplaza el deprecated `new Date()`. (3) Removal de varias APIs legacy que fueron deprecadas por multiples major versions. (4) Sigue el schedule de LTS: v26 sera Current hasta v28 es lanzado (Apr 2027), luego v26 sera LTS durante 12 meses.
- **Accion requerida**: Monitorear
- **Detalles**: Node.js 26 es el "Current" release, no LTS. Para produccion estable, seguir usando Node.js 22 LTS o esperar a Node.js 28 LTS (2027). El upgrade de V8 a 14.6 afecta JIT compilation, garbage collection, y soporte de nuevas features JS. El Temporal API habilitado por defecto puede causar issues si el codigo espera el `Date` object clasico o depende de comportamiento temporal especifico. Deadline critico: Node.js 20 EOL en 30 de abril ya paso (hoy es 17 de mayo), las aplicaciones en Node 20 ya no reciben security patches.

---

## [2026-05-06] React 19.2.6 patch release - Type hardening y performance improvements

- **Fuente**: [React GitHub Releases](https://github.com/facebook/react/releases), [endoflife.date React](https://endoflife.date/react)
- **Confianza**: Alta
- **Que cambia**: React 19.2.6 lanzado el 6 de mayo 2026 (junto con 19.1.7 y 19.0.6). Cambios: (1) Type hardening — mejoras en inferencia de tipos TypeScript para componentes y hooks. (2) Performance improvements — optimizaciones en reconciliation y rendering.
- **Accion requerida**: Monitorear
- **Detalles**: Esta es una patch release, no breaking changes. El type hardening es beneficial para proyectos TypeScript. React 19.2.x es stable para produccion. No hay React 20 aun — la version estable es 19.2.x.

---

## [2026-05-17] Python 3.10 End-of-Life - October 31, 2026 (5 meses)

- **Fuente**: [Python devguide oficial](https://devguide.python.org/versions/), [HeroDevs EOL tracker](https://www.herodevs.com/blog-posts/python-end-of-life-dates-every-versions-support-timeline), [Heroku deprecation](https://devcenter.heroku.com/changelog-items/3490)
- **Confianza**: Alta
- **Que cambia**: Python 3.10 alcanza End-of-Life el 31 de octubre 2026 (en 5 meses y 14 dias). Ya no estara en security-fix mode — solo updates son para vulnerabilidades criticas. Ecosistema acelerando: major frameworks como pandas, numpy, y otros estan removiendo support para 3.10 de sus test matrices CI/CD.
- **Accion requerida**: Monitorear/Actualizar (8 meses)
- **Detalles**: Python 3.10 lanzado en October 2021 con ciclo de 5 anos. Proyectos que aun soportan 3.10 deben planear migracion a 3.11+ antes de Oct 31. Si una dependencia dropa soporte 3.10, auto-pierdes security fixes para esa libreria tambien, creando vulnerabilidad transitiva. AWS Lambda, Heroku, y otros proveedores cloud ya estan deprecando 3.10 en sus runtimes. Target minimo: Python 3.11 (EOL Oct 2027).
