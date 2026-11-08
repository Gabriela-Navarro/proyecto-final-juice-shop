# Diagnóstico de madurez inicial (OWASP SAMM)

Aplicación: OWASP Juice Shop (fork: proyecto-final-juice-shop)
Marco: OWASP SAMM 2.0 — 5 funciones, 15 prácticas, escala 0–3.

Se revisaron uno por uno los archivos del repositorio para asignar cada nivel. Si no se encontró evidencia, se puso 0. Las prácticas con objetivo 0 no se van a mejorar porque quedan fuera de lo que cubre este proyecto.

| Práctica SAMM | Nivel actual | Nivel objetivo | Evidencia del nivel actual |
|---|---|---|---|
| Estrategia y métricas | 0 | 1 | No hay plan ni metas de seguridad |
| Políticas y cumplimiento | 0 | 1 | `SECURITY.md` solo dice cómo reportar vulnerabilidades, no tiene reglas de seguridad para el desarrollo |
| Educación y guía | 0 | 0 | No hay guías para programar seguro. Objetivo 0: el proyecto se enfoca en el pipeline |
| Evaluación de amenazas | 0 | 2 | `threat-model.json` tiene un diagrama (DFD STRIDE, 31 elementos) pero no identifica ninguna amenaza. Objetivo 2: agregar 12+ amenazas (Fase 2) |
| Requisitos de seguridad | 0 | 1 | No hay requisitos de seguridad documentados. Objetivo 1: crear 10+ requisitos ASVS (Fase 1) |
| Arquitectura de seguridad | 0 | 1 | No hay diseño de seguridad ni clasificación de datos. Objetivo 1: crear diagrama de arquitectura y DFD (Fase 1) |
| Construcción segura | 1 | 2 | `codeql-analysis.yml` busca vulnerabilidades en cada push y PR (`security-extended`), con permisos mínimos y acciones fijadas por SHA/@v3. No bloquea el merge. Objetivo 2: agregar SCA, SBOM y gate de bloqueo (Fases 3–4) |
| Despliegue seguro | 1 | 2 | Dockerfile usa imagen mínima (distroless) y usuario sin permisos de administrador (65532). Objetivo 2: agregar escaneo de IaC e imagen (Fase 5) |
| Gestión de defectos | 0 | 1 | Hay plantillas de issues pero no hay proceso para manejar vulnerabilidades. Objetivo 1: centralizar hallazgos y corregir 5 vía PR (Fase 7) |
| Evaluación de arquitectura | 0 | 0 | No hay revisión de arquitectura. Objetivo 0: la arquitectura viene del proyecto original |
| Pruebas basadas en requisitos | 0 | 0 | Hay pruebas funcionales pero ninguna verifica seguridad. Objetivo 0: no alcanza a crear pruebas por cada requisito |
| Pruebas de seguridad | 0 | 1 | No hay pruebas con la app corriendo (DAST). CodeQL es análisis estático y cuenta en Construcción segura. Objetivo 1: agregar ZAP (Fase 6) |
| Gestión de incidentes | 1 | 1 | `SECURITY.md` dice cómo reportar vulnerabilidades, con correo y PGP |
| Gestión de entornos | 0 | 0 | No hay ambientes separados (dev/staging/producción). Objetivo 0: no hay infraestructura real |
| Gestión operativa | 0 | 0 | Existe un dashboard de Grafana pero es de la app, no de seguridad. Objetivo 0: se necesitaría la app en producción |

Nota: `.dependabot/config.yml` usa formato antiguo (v1) y apunta a la rama `develop` que no existe. No se considera evidencia.

Nota metodológica: `SECURITY.md` cuenta para Gestión de incidentes, no para Políticas y cumplimiento. CodeQL cuenta para Construcción segura, no para Pruebas de seguridad, porque analiza código sin correr la app.
