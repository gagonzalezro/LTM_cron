# May 2026 Research Findings

## [2026-05-05] Node.js 26 Released - Temporal API enabled by default, breaking changes

- **Source**: [nodejs.org Release Notes](https://nodejs.org/en/blog/release/v26.0.0) / [Help Net Security](https://www.helpnetsecurity.com/2026/05/07/node-js-26-released/)
- **Confidence**: Alta
- **What changes**: Node.js 26.0.0 (5 mayo 2026) introduce cambios breaking importantes: (1) **Temporal API habilitado por defecto** — reemplaza el legado Date object con una API moderna para date/time. (2) **Remoción de módulos _stream_* privados**: `_stream_wrap`, `_stream_readable`, `_stream_writable`, `_stream_duplex`, `_stream_transform`, `_stream_passthrough` — código que require directamente estos módulos rompe. (3) **Remoción de `http.Server.prototype.writeHeader()`** — usar `writeHead()`. (4) **Remoción de `--experimental-transform-types`**. (5) **V8 14.6** engine update. (6) **Undici 8.0** update. (7) **`module.register()` runtime-deprecated**.
- **Action required**: Actualizar dependencia
- **Details**: El Temporal API es adición (no breaking), pero los cambios en streams y http APIs son rompedores. Cualquier paquete que require los módulos _stream_* directamente falla en Node.js 26. Node.js 26 entra en LTS en octubre 2026 — no es urgente actualizar ahora a menos que se necesite Temporal API. Migración: reemplazar cualquier require directo de _stream_* con las versiones públicas (import `readable-stream` del npm). Revisar package.json para dependencias que podrían usar estas APIs internas.

---

## [2026-05-07] Python 3.15.0 Beta 1 Released - Feature freeze, lazy imports, improved JIT

- **Source**: [Python Insider Blog](https://blog.python.org/2026/05/python-3150-beta-1/) / [PEP 790 Release Schedule](https://peps.python.org/pep-0790/)
- **Confidence**: Alta
- **What changes**: Python 3.15.0 Beta 1 (7 mayo 2026) marca el feature freeze — no se añadirán features nuevas después de este punto. Cambios principales: (1) **Lazy imports** — módulos se importan solo cuando se usan, deferiendo el costo a ejecución, acelera startup. (2) **Improved JIT compiler** — 8-9% performance boost en x86-64 Linux, 12-13% en Apple silicon macOS. (3) **Revert a generational GC** — Python 3.15 vuelve al generational garbage collector de Python 3.13 (Python 3.14 usaba el nuevo incremental cycle GC). (4) **Zero-overhead sampling profiler**. (5) **UTF-8 by default** en modo texto. (6) **Stable ABI para free-threaded CPython**.
- **Action required**: Monitorear
- **Details**: El feature freeze significa que desde el 7 de mayo, solo bug fixes y documentación — no hay cambios API después de esto. La versión final de Python 3.15 se espera para **1 de octubre de 2026**. El cambio a lazy imports podría impactar aplicaciones que dependan del comportamiento de importación eager — verificar si hay side effects en módulos importados. El revert al generational GC es buena noticia para aplicaciones con memoria presionada (mejor predictability). Python 3.10 alcanza EOL el 31 octubre 2026 — comenzar migración si aún se usa.

---

## [2026-05-12] Microsoft May 2026 Patch Tuesday - 120 vulnerabilidades, incluyendo CVE-2026-41096 DNS RCE crítico

- **Source**: [Microsoft Security Update Guide](https://msrc.microsoft.com/update-guide/releaseNoteIndex) / [Krebs on Security](https://krebsonsecurity.com/2026/05/patch-tuesday-may-2026-edition/) / [Help Net Security](https://www.helpnetsecurity.com/2026/05/12/microsoft-may-2026-patch-tuesday/)
- **Confidence**: Alta
- **What changes**: Microsoft publicó el 12 de mayo de 2026 actualizaciones de seguridad para **120 vulnerabilidades totales** (17 "Critical", 14 RCE, 2 escalación, 1 info disclosure). Las más importantes: (1) **CVE-2026-41096** (CVSS 9.8) — Windows DNS Client RCE heap buffer overflow no autenticado. Un atacante envía un DNS response crafteado que corrompe la memoria del DNS Client, resultando en RCE sobre la red sin autenticación. Afecta la base amplia: workstations, laptops, servidores, VMs, cloud Windows. (2) **CVE-2026-41089** (CVSS 9.8) — Netlogon stack buffer overflow en domain controllers, RCE no autenticado preauthentication — wormable. (3) **CVE-2026-42898** (CVSS 9.9) — Microsoft Dynamics 365 RCE. (4) **CVE-2026-40402** (CVSS 9.3) — Hyper-V privilege escalation: guest VM no privilegiado puede escalar y acceder al host. (5) **CVE-2026-40379** (CVSS 9.3) — Enterprise Security Token Service spoofing.
- **Action required**: Urgente
- **Details**: Aplicar inmediatamente estos parches, especialmente CVE-2026-41096 (DNS RCE) y CVE-2026-41089 (Netlogon). El DNS RCE es especialmente peligroso porque la resolución DNS es omnipresente — todos los sistemas Windows tienen el DNS Client. El Netlogon RCE afecta específicamente domain controllers que no fueron parcheados. Inusual: **no hay zero-days** en este Patch Tuesday — es la primera vez en ~2 años. Sin embargo, la severidad es máxima — establecer actualización como crítica. Algunos sistemas experimentaron fallos de instalación con code 0x800f0922 (bajo espacio en EFI partition <10 MB) — Microsoft mitigó con Known Issue Rollback (KIR).

---

## [2026-05-13] CVE-2026-42945 NGINX Rift - Buffer overflow 18 años oculto, RCE + DoS exploited activamente

- **Source**: [Akamai Security Research](https://www.akamai.com/blog/security-research/nginx-critical-heap-buffer-overflow-cve-2026-42945) / [Axonius Security Alert](https://www.axonius.com/blog/cve-2026-42945-nginx-rift) / [AlmaLinux Blog](https://almalinux.org/blog/2026-05-13-nginx-rift-cve-2026-42945/)
- **Confidence**: Alta
- **What changes**: CVE-2026-42945, bautizado "NGINX Rift" y divulgado el 13 de mayo de 2026, es un heap buffer overflow en `ngx_http_rewrite_module` que ha estado presente en **todo build de NGINX desde 2008** (~18 años). CVSS 9.2 (muy alto). El trigger: rewrite directive con unnamed regex capture ($1, $2) + replacement string con ? seguido de otra rewrite/if/set directive. Un atacante remoto no autenticado envía un URI crafteado que corrompe el heap del worker process, permitiendo **RCE o DoS confiable**. Afecta: NGINX Open Source 0.6.27–1.30.0; NGINX Plus vR32–R36.
- **Action required**: Actualizar dependencia
- **Details**: Versiones parcheadas: NGINX 1.30.1, 1.31.0 (open source); NGINX Plus R36 P4, R32 P6. Las canary systems de VulnCheck comenzaron a detectar intentos de explotación el **16 de mayo** (3 días después de la divulgación pública), indicando que exploits están siendo desarrollados activamente. Este es un vulnerability de altísima prioridad para cualquier organización con NGINX expuesto en internet. La configuración típica (rewrite + captura) es extremadamente común — esto probablemente afecta millones de deployments globales. Sin exposición de internet: usar WAF/firewall para bloquear requests con patterns sospechosos mientras se actualiza.

---

## [2026-05-14] CVE-2026-42897 Exchange Server OWA - Spoofing via XSS, explotación activa (sin parche permanente)

- **Source**: [Microsoft Community Hub](https://techcommunity.microsoft.com/blog/exchange/addressing-exchange-server-may-2026-vulnerability-cve-2026-42897/4518498) / [The Hacker News](https://thehackernews.com/2026/05/on-prem-microsoft-exchange-server-cve.html) / [Help Net Security](https://www.helpnetsecurity.com/2026/05/15/exchange-server-cve-2026-42897-exploited/)
- **Confidence**: Alta
- **What changes**: CVE-2026-42897 (CVSS 8.1) divulgado el 14 de mayo de 2026 — vulnerabilidad en Outlook Web Access (OWA) de Exchange Server on-premises. El defecto: cross-site scripting (XSS) que permite un atacante enviar un email crafteado; si el usuario lo abre en OWA, código JavaScript arbitrario se ejecuta en el browser. Resultante: spoofing (el email parece venir de otra persona), robo de credenciales de sesión, acceso a mailbox de otras personas. Afecta: Exchange Server 2016, 2019, Subscription Edition (SE) — **NO Exchange Online**. Explotación activa confirmada. El 15 de mayo, CISA añadió a KEV con deadline para agencias federales de FCEB el 29 de mayo de 2026.
- **Action required**: Urgente
- **Details**: Microsoft lanzó un parche temporal automático (EM Service mitigation) que se activó automáticamente para clientes con Exchange EM Service habilitado. **Sin parche permanente oficial al 24 de mayo de 2026** — solo mitigaciones y parches temporales en desarrollo. Recomendación: (1) Aplicar el parche temporal de Microsoft (si disponible en tu versión), (2) Monitorear anuncios de Microsoft para el parche permanente, (3) Revisar logs de OWA para acceso sospechoso post-disclosure (14+ mayo). El deadline de CISA (29 mayo) se acerca rápidamente — agencias federales deben remediadores antes de esa fecha.

---

## [2026-05-18] AWS Lambda Node.js 20 End of Life - Tres fases de deprecación, última actualización 1 julio 2026

- **Source**: [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) / [CloudQuery Blog](https://www.cloudquery.io/blog/aws-lambda-nodejs-20-eol)
- **Confidence**: Alta
- **What changes**: AWS Lambda implementa el EOL de Node.js 20 upstream (30 abril 2026) en **tres fases**: (1) **30 abril 2026** — AWS deja de aplicar security patches y updates a Node.js 20.x runtime; las funciones ya no son elegibles para technical support de AWS. (2) **1 junio 2026** — no se pueden crear **nuevas** funciones Lambda con Node.js 20.x runtime. (3) **1 julio 2026** — no se pueden **actualizar** funciones existentes con Node.js 20.x runtime. Las invocaciones de funciones con Node.js 20.x **nunca** son bloqueadas — solo el deployment es bloqueado.
- **Action required**: Urgente
- **Details**: Si tu stack incluye funciones Lambda con Node.js 20.x, **migrar a Node.js 22 antes del 1 de julio 2026** (~33 días). Acciones: (1) Auditar: `aws lambda list-functions --query 'Functions[?Runtime==\`nodejs20.x\`]'`. (2) Probar tu código con Node.js 22 localmente. (3) Actualizar la definición de función (SAM template, CDK, CloudFormation, etc.). (4) Redeployar. Breaking changes Node.js 20→22: import assertions (`assert`) → import attributes (`with`), require native addons re-build, streams high-water marks aumentado (16KB→64KB). Complementa la entrada anterior del long-term-memory sobre Node.js 20 EOL el 30 abril — esta es la fase 2 de la implementación en AWS.

---

## [2026-05-24] Docker Engine 29 & 28 Breaking Changes - rootless networking, iptables refactoring

- **Source**: [Docker Engine 29 Release Notes](https://docs.docker.com/engine/release-notes/29/) / [Docker Engine 28 Release Notes](https://docs.docker.com/engine/release-notes/28.0/)
- **Confidence**: Alta
- **What changes**: Dos releases de Docker con cambios breaking: **Docker Engine 29**: (1) **Rootless network driver changed** — gvisor-tap-vsock ahora es el driver por defecto (antes slirp4netms). `slirp4netms` ya no se instala vía Docker packaging. (2) **Private time namespace habilitado por defecto** en containers en kernels soportados. **Docker Engine 28** (más impactante): (3) **iptables refactoring extensive** — las reglas `iptables` e `ip6tables` usadas para port publishing e network isolation fueron reescritas: primer paso para futura soporte nativa de `nftables`. (4) **Port access restrictions** — acceso routed directo a puertos de container no expuestos via `-p/--publish` ahora está bloqueado en la chain DOCKER. (5) **Feature removal**: `windows-dns-proxy` flag removido (introducido en 26.1.0 para Windows containers). (6) **Kernel requirement**: `dockerd` ahora requiere soporte `ipset` en el kernel Linux.
- **Action required**: Monitorear
- **Details**: El cambio de rootless driver (Engine 29) es benéfico — gvisor-tap-vsock es más performante y seguro. El iptables refactoring (Engine 28) es más delicado: si depende de comportamiento específico de iptables, verificar que los containers aún accedan correctamente. La restricción de port access directo es mejoría de seguridad pero podría romper herramientas que bypassed `-p/--publish` via port binding directo. Verificar: `kernel version >=5.10` para soporte `ipset`, y verificar que las políticas de firewall/iptables personalizadas aún funcionan después de actualizar. Docker Swarm: volúmenes legacy pueden perder acceso en algunos casos — mantener actualización de storage drivers.

---
