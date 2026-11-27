# Análisis de Hallazgos DAST (OWASP ZAP)

**Proyecto:** OWASP Juice Shop - Ciclo de Desarrollo Seguro (SSDLC)
**Autoras:** Gabriela Navarro y Sandra Soria
**Fecha:** Noviembre 2026
**Versión:** 1.0
**Fase:** Fase 6 - Pruebas Dinámicas (DAST)

Tras ejecutar ZAP Baseline en el pipeline, se identificaron 2 alertas de riesgo Medio y 5 de riesgo Bajo. A continuación, se analizan los 5 hallazgos más relevantes.

| # | Alerta (ZAP) | Riesgo | Descripción del hallazgo | Recomendación de mitigación |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **Content Security Policy (CSP) Header Not Set** | Medio | La app no define una política de CSP, facilitando ataques XSS (CWE-693). | Configurar el encabezado `Content-Security-Policy` en el servidor. |
| 2 | **Cross-Domain Misconfiguration** | Medio | El servidor permite peticiones desde cualquier origen (`Access-Control-Allow-Origin: *`), exponiendo APIs (CWE-264). | Restringir los orígenes permitidos (CORS) a dominios confiables. |
| 3 | **Cross-Origin-Embedder-Policy Header Missing** | Bajo | Falta la cabecera COEP, que previene que recursos cruzados no autorizados se carguen. | Agregar el encabezado `Cross-Origin-Embedder-Policy: require-corp`. |
| 4 | **Dangerous JS Functions** | Bajo | Se detectó el uso de `bypassSecurityTrustHtml` en Angular, lo que puede anular la sanitización (CWE-749). | Evitar el uso de esta función o asegurarse de sanitizar manualmente el HTML. |
| 5 | **Full Path Disclosure** | Bajo | El servidor expone rutas internas del sistema en las respuestas, como `/ftp/package.json.bak` (CWE-209). | Configurar el servidor web para que devuelva errores genéricos y oculte rutas internas. |