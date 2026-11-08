# Diagnóstico de madurez inicial (OWASP SAMM)

Aplicación: OWASP Juice Shop (fork: proyecto-final-juice-shop)
Marco: OWASP SAMM 2.0 — 5 funciones, 15 prácticas, escala 0–3.

Se evaluaron las 15 prácticas de SAMM revisando uno por uno los archivos del repositorio: workflows de `.github/workflows/`, Dockerfile, `SECURITY.md`, `threat-model.json`, `.dependabot/config.yml`, `CONTRIBUTING.md`, `test/`, `monitoring/` y la documentación general. Cuando no se encontró evidencia, o la evidencia existente no cumplía el criterio de SAMM para el nivel 1, se asignó 0 y se justificó en la columna de evidencia. Las prácticas con objetivo 0 se declaran fuera del alcance del proyecto (fases 1–8) y no se mejorarán.

| Práctica SAMM | Nivel actual | Nivel objetivo | Evidencia del nivel actual |
|---|---|---|---|
| Estrategia y métricas | 0 | 1 | No hay plan ni metas de seguridad. `frontend-bundle-analysis.yml` mide tamaño de bundle, no seguridad. Objetivo 1: medir hallazgos y tiempos de remediación vía DefectDojo (Fase 7) |
| Políticas y cumplimiento | 0 | 1 | `SECURITY.md` dice cómo reportar vulnerabilidades, pero no hay reglas de seguridad para el desarrollo ni `politica-de-bloqueo.md`. Objetivo 1: `politica-de-bloqueo.md` + protección de rama con checks obligatorios (Fase 7) |
| Educación y guía | 0 | 0 | `CONTRIBUTING.md` tiene reglas para contribuir pero no enseña a programar seguro; ESLint no tiene plugins de seguridad. Objetivo 0: el proyecto se enfoca en el pipeline, no en capacitación |
| Evaluación de amenazas | 0 | 2 | `threat-model.json` tiene un diagrama de flujo de datos (DFD STRIDE) con 31 elementos, pero 0 amenazas identificadas. Objetivo 2: agregar 12+ amenazas con mitigación y trazabilidad (Fase 2) |
| Requisitos de seguridad | 0 | 1 | No hay lista de requisitos de seguridad. `swagger.yml` describe la API, no es un requisito de seguridad. Objetivo 1: 10+ requisitos ASVS nivel 1/2 (Fase 1) |
| Arquitectura de seguridad | 0 | 1 | README describe la arquitectura general pero no hay diseño de seguridad ni clasificación de datos. Objetivo 1: arquitectura, DFD y clasificación de datos (Fase 1) |
| Construcción segura | 1 | 2 | `codeql-analysis.yml` analiza el código en cada push y PR buscando vulnerabilidades (`security-extended`), con permisos mínimos. `actions/checkout` fijada por SHA; CodeQL fijada a @v3. No bloquea el merge si encuentra algo. El workflow excluye `data/static/codefixes` y la regla `js/missing-rate-limiting`; esa exclusión se documentará como excepción en la Fase 7. Objetivo 2: añadir SCA + SBOM + Dependabot moderno y gate de bloqueo (Fases 3 y 4) |
| Despliegue seguro | 1 | 2 | Dockerfile usa imagen mínima (distroless) y corre sin administrador (usuario 65532). No hay escaneo de infraestructura como código. Objetivo 2: escaneo de IaC e imagen con Checkov/Trivy (Fase 5) |
| Gestión de defectos | 0 | 1 | `.github/ISSUE_TEMPLATE/` tiene plantillas de bugs y features, pero no hay plantilla de seguridad ni proceso para priorizar y corregir vulnerabilidades. Objetivo 1: centralizar en DefectDojo, corregir 5 hallazgos vía PR y documentar un VEX (Fase 7) |
| Evaluación de arquitectura | 0 | 0 | No hay revisión formal de arquitectura. Objetivo 0: la arquitectura viene del proyecto original, no la diseñamos nosotras; el alcance cubre TA, no AA |
| Pruebas basadas en requisitos | 0 | 0 | Hay pruebas funcionales en `test/` y Cypress, pero ninguna verifica un requisito de seguridad. Objetivo 0: se definen requisitos ASVS pero no alcanza a crear pruebas trazadas para cada uno |
| Pruebas de seguridad | 0 | 1 | No hay pruebas sobre la app corriendo (DAST/ZAP). CodeQL analiza código estático y cuenta en Construcción segura. Objetivo 1: ZAP baseline en cada PR + full scan local/autenticado (Fase 6) |
| Gestión de incidentes | 1 | 1 | `SECURITY.md` explica cómo reportar vulnerabilidades, incluyendo correo con PGP. Se mantiene el nivel; no se profundiza |
| Gestión de entornos | 0 | 0 | Existen `docker-compose.test.yml` y `.devcontainer.json` pero no hay ambientes separados (dev/staging/producción) con controles de acceso. Objetivo 0: no hay infraestructura real que gestionar |
| Gestión operativa | 0 | 0 | Existe un dashboard de Grafana (`monitoring/grafana-dashboard.json`) pero es monitoreo funcional, no de seguridad, y no hay clasificación de datos. Objetivo 0: se necesitaría la app en producción y monitoreo de seguridad |

Nota sobre Dependabot: `.dependabot/config.yml` usa el formato antiguo (v1) y apunta a la rama `develop`, que no existe en el fork. No existe `.github/dependabot.yml` (v2). GitHub ya no procesa el formato v1, por lo que no se considera evidencia de gestión automatizada de dependencias.

Nota metodológica: `SECURITY.md` se clasifica como política de divulgación de vulnerabilidades (Gestión de incidentes), no como política de seguridad del SDLC (Políticas y cumplimiento). CodeQL se clasifica como SAST (Construcción segura) y no como prueba de seguridad (Pruebas de seguridad), porque analiza código estático y no ejecuta la aplicación. Las prácticas con objetivo 0 quedan fuera del alcance porque las actividades necesarias para subirlas no forman parte de las fases 1–8 del proyecto.
