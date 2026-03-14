# Seguridad: OWASP Top 10

El **OWASP Top 10** es una lista de los riesgos de seguridad más críticos en aplicaciones web, publicada y actualizada por el proyecto OWASP (Open Web Application Security Project). Conocerlos ayuda a priorizar controles en diseño, desarrollo y despliegue.

## Resumen OWASP Top 10 (referencia actual)

| Riesgo                                             | Descripción breve                                                 | Buenas prácticas                                                                                                 |
| -------------------------------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **A01 Broken Access Control**                      | Fallos que permiten acceder a recursos o acciones no autorizadas. | Autorización en cada endpoint; no confiar en ocultar en UI; validar IDs y pertenencia.                           |
| **A02 Cryptographic Failures**                     | Datos sensibles expuestos por cifrado débil o ausente.            | Cifrar en tránsito (TLS) y en reposo; no guardar datos sensibles innecesarios; algoritmos y claves fuertes.      |
| **A03 Injection**                                  | Inyección SQL, de comandos, etc.                                  | Consultas parametrizadas; evitar concatenar entrada de usuario; ORM y APIs seguras.                              |
| **A04 Insecure Design**                            | Riesgos por diseño (flujos, modelos de amenaza).                  | Modelado de amenazas; principios de mínimo privilegio y defensa en profundidad.                                  |
| **A05 Security Misconfiguration**                  | Configuración por defecto insegura, headers, mensajes de error.   | Hardening; quitar servicios y páginas por defecto; headers de seguridad (CSP, HSTS, etc.).                       |
| **A06 Vulnerable and Outdated Components**         | Dependencias con vulnerabilidades conocidas.                      | Inventario de dependencias; actualizar y parchear; usar herramientas (Snyk, Dependabot, OWASP Dependency-Check). |
| **A07 Identification and Authentication Failures** | Fallos en login, sesiones, credenciales.                          | MFA; contraseñas fuertes y no por defecto; gestión segura de sesiones; límite de intentos.                       |
| **A08 Software and Data Integrity Failures**       | Código o datos manipulados (CI/CD, deserialización).              | Firmar artefactos; verificar integridad; no deserializar datos no confiables sin validación.                     |
| **A09 Security Logging and Monitoring Failures**   | Falta de registro o detección de incidentes.                      | Logs de autenticación y acceso; alertas ante anomalías; no registrar datos sensibles en claro.                   |
| **A10 Server-Side Request Forgery (SSRF)**         | La aplicación hace peticiones a URLs controladas por el atacante. | Validar y restringir destinos (whitelist); no reenviar cabeceras sensibles; segmentar red.                       |

La lista se revisa periódicamente; conviene consultar la versión actual en [owasp.org](https://owasp.org/Top10/). Integrar estas prácticas en diseño, code review y pruebas de seguridad reduce la superficie de ataque.

---

[← Volver al README principal](../../README.md)
