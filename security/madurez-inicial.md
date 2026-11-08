# Diagnóstico de madurez inicial (OWASP SAMM)

Aplicación: OWASP Juice Shop (fork: proyecto-final-juice-shop)

Escala de 0 a 3. El nivel actual se basa en lo que existe en el repositorio antes de iniciar el proyecto.

| Práctica SAMM | Nivel actual | Nivel objetivo | Evidencia del nivel actual |
|---|---|---|---|
| Estrategia y métricas | 0 | 1 | Sin evidencia |
| Políticas y cumplimiento | 0 | 1 | Sin evidencia |
| Educación y guía | 0 | 0 | Sin evidencia |
| Evaluación de amenazas | 0 | 1 | No existe modelo de amenazas |
| Requisitos de seguridad | 0 | 1 | No hay requisitos documentados |
| Arquitectura de seguridad | 0 | 1 | Sin evidencia |
| Construcción segura | 1 | 2 | `codeql-analysis.yml` corre en el build, con checkout fijado por SHA y permisos mínimos |
| Despliegue seguro | 0 | 1 | Sin evidencia |
| Gestión de defectos | 0 | 1 | Sin evidencia |
| Evaluación de arquitectura | 0 | 0 | Sin evidencia |
| Pruebas basadas en requisitos | 0 | 1 | Sin evidencia |
| Pruebas de seguridad | 1 | 2 | CodeQL `security-extended` en cada push y PR, pero no bloquea nada |
| Gestión de incidentes | 1 | 1 | `SECURITY.md` explica cómo reportar vulnerabilidades |
| Gestión de entornos | 0 | 1 | Sin evidencia |
| Gestión operativa | 0 | 1 | No hay clasificación de datos |

Nota: `.dependabot/config.yml` usa el formato antiguo y apunta a la rama `develop`, por lo que no se considera evidencia de revisión automática de dependencias.
