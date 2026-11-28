# Modelo de amenazas y matriz de trazabilidad

**Proyecto:** OWASP Juice Shop 
**Autoras:** Gabriela Navarro y Sandra Soria
**Fecha:** Noviembre 2026
**Fase:** Fase 2 - Modelado de Amenazas

---

Se utilizó el DFD de la Fase 1 (`security/diseño/dfd.md`) como base para construir el modelo de amenazas en OWASP Threat Dragon y aplicar STRIDE. El archivo completo del modelo se encuentra en `security/threat-model/juice-shop-threat-model.json`.

## Escala utilizada

| Nivel | Impacto | Probabilidad |
|---|---|---|
| Bajo | Daño menor, no afecta datos ni el funcionamiento del sistema | Difícil de explotar |
| Medio | Afecta una función o un grupo reducido de usuarios | Requiere cierto conocimiento técnico o herramientas específicas |
| Alto | Afecta datos sensibles o el funcionamiento del sistema | Se puede explotar con herramientas comunes |
| Crítico | Compromete por completo el sistema o los datos de todos los usuarios | Ya existe evidencia confirmada de que es explotable |

La prioridad de cada amenaza se determina según el impacto y la probabilidad de cada amenaza.


## Tabla de amenazas

| # | STRIDE | Amenaza | Componente | Impacto | Probabilidad | Prioridad | Mitigación |
|---|---|---|---|---|---|---|---|
| 1 | Spoofing | Falta de límite de intentos de inicio de sesión | Servicio de Autenticación JWT | Alto | Alta | Alta | Bloquear la cuenta tras 5 intentos fallidos de inicio de sesión |
| 2 | Spoofing | Llave JWT hardcodeada permite falsificar tokens | Servicio de Autenticación JWT | Crítico | Alta | Alta | Guardar la llave JWT de forma segura en los secretos del CI, sin incluirla en el código fuente. |
| 3 | Tampering | Inyección SQL en el login | API REST Node.js | Crítico | Alta | Alta | Utilizar consultas parametrizadas para evitar que los datos ingresados por el usuario puedan modificar la consulta SQL. |
| 4 | Tampering | Falta de validación del precio final en el servidor | API REST Node.js | Alto | Media | Alta | Validar precios, cantidades y cupones en el servidor antes de cobrar |
| 5 | Tampering | Redirección a sitios web maliciosos | API REST Node.js | Medio | Media | Media | Verificar que la URL de destino se encuentre dentro de una lista de sitios permitidos |
| 6 | Repudiation | Modificación o eliminación de reseñas de otros usuarios | MarsDB | Medio | Alta | Media | Verificar que la reseña pertenezca al usuario antes de permitir su edición o eliminación, y guardar un registro de la acción realizada |
| 7 | Information Disclosure | La inyección SQL permite leer toda la base de datos | SQLite | Crítico | Alta | Alta | Utilizar consultas parametrizadas para evitar el acceso no autorizado a los datos de la base de datos. |
| 8 | Information Disclosure | Exposición de información interna del servidor en mensajes de error | API REST Node.js | Medio | Alta | Media | Mostrar mensajes de error genéricos al usuario final |
| 9 | Denial of Service | Falta de validación al subir fotos de perfil | API REST Node.js | Alto | Media | Media | Limitar el tamaño y validar el tipo de archivo antes de guardarlo |
| 10 | Denial of Service | Dependencia de servicios externos sin control de fallas | API REST Node.js | Medio | Media | Media | Establecer un tiempo máximo de espera y manejar los errores cuando un servicio externo no esté disponible |
| 11 | Elevation of Privilege | Acceso no autorizado a funciones de administrador | API REST Node.js | Crítico | Alta | Alta | Validar el rol del usuario en el backend para cada endpoint de administración |
| 12 | Elevation of Privilege | Ejecución de código malicioso en el perfil de usuario | API REST Node.js | Crítico | Media | Alta | Eliminar el uso de eval() y procesar el nombre de usuario únicamente como texto |

## Matriz de trazabilidad

| # | Amenaza | Requisito ASVS | Control del pipeline |
|---|---|---|---|
| 1 | Falta de límite de intentos de inicio de sesión | REQ-02 | Revisión manual |
| 2 | Llave JWT hardcodeada permite falsificar tokens | REQ-07 | SAST |
| 3 | Inyección SQL en el login | REQ-06 | SAST |
| 4 | Falta de validación del precio final en el servidor | REQ-10 | Revisión manual |
| 5 | Redirección a sitios web maliciosos | REQ-06 | SAST |
| 6 | Modificación o eliminación de reseñas de otros usuarios | REQ-05 | Revisión manual y DAST autenticado |
| 7 | La inyección SQL permite leer toda la base de datos | REQ-06 | SAST |
| 8 | Exposición de información interna del servidor en mensajes de error | REQ-08 | DAST |
| 9 | Falta de validación al subir fotos de perfil | REQ-11 | DAST y revisión manual |
| 10 | Dependencia de servicios externos sin control de fallas | Sin requisito ASVS definido en la Fase 1 | Revisión manual |
| 11 | Acceso no autorizado a funciones de administrador | REQ-05 | Revisión manual y DAST autenticado |
| 12 | Ejecución de código malicioso en el perfil de usuario | REQ-06 | SAST |

### Notas

- La amenaza 2 se relacionó con el REQ-07 por ser el requisito de criptografía más cercano, aunque no contempla la protección de llaves JWT.
- La amenaza 10 no tiene un requisito asociado porque la Fase 1 no contempla las fallas de servicios externos.