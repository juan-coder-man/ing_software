# Seguridad en APIs y datos sensibles

La seguridad en el desarrollo de software abarca la protección de APIs, el manejo correcto de datos sensibles y el cumplimiento de normativas. Diseñar con seguridad desde el inicio y aplicar estándares reconocidos reduce riesgos y costes de remediación.

## Protección de APIs

### Fuerza bruta

Medidas para limitar intentos de acceso por prueba y error:

- **Rate limiting:** límite de solicitudes por IP o por usuario en una ventana de tiempo.
- **Bloqueo temporal** tras varios intentos fallidos (login, recuperación de contraseña).
- **CAPTCHA** en formularios críticos para distinguir usuarios reales de bots.
- **Autenticación multifactor (MFA)** para cuentas sensibles.
- **Monitoreo de IPs sospechosas** y listas de bloqueo si aplica.

### CORS (Cross-Origin Resource Sharing)

CORS es una política del navegador que controla **qué dominios** pueden consumir la API desde el frontend. Si se configura mal (por ejemplo permitiendo `*`), la API puede quedar expuesta a orígenes no confiables y facilitar fugas de datos o uso indebido. Debe restringirse a los orígenes permitidos (whitelist de dominios).

### Contraseñas

- **Nunca** almacenar contraseñas en texto plano.
- Usar **hash fuerte con salt** (bcrypt, Argon2, scrypt).
- Aplicar **políticas de complejidad** (longitud mínima, caracteres especiales, etc.) y no reutilizar contraseñas cuando el estándar lo exija.

## Datos sensibles

### Qué se considera sensible

Contraseñas, tokens, datos personales (nombre, documento, correo), información financiera y cualquier dato que identifique o comprometa al usuario o al negocio.

### Buenas prácticas en respuestas de API

- No exponer datos innecesarios; devolver solo lo que el cliente necesita.
- Cifrar información sensible en tránsito (HTTPS) y en reposo cuando aplique.
- Aplicar **mínimo privilegio:** cada endpoint y rol solo accede a lo estrictamente necesario.

### Normativas y estándares

| Referencia                       | Ámbito                                                   |
| -------------------------------- | -------------------------------------------------------- |
| **OWASP**                        | Buenas prácticas y Top 10 para aplicaciones web y APIs   |
| **ISO 27001**                    | Sistema de gestión de la seguridad de la información     |
| **GDPR**                         | Protección de datos personales en la UE (si aplica)      |
| **PCI-DSS**                      | Datos de tarjetas y entornos de pago                     |
| Buenas prácticas de **API REST** | Diseño seguro de endpoints, autenticación y autorización |

## Privacidad por diseño

El principio de **privacidad por diseño** consiste en integrar la protección de datos **desde el diseño** del sistema, no como una capa añadida después. En APIs implica:

- Minimizar los datos recolectados y almacenados.
- Cifrar comunicaciones y datos sensibles por defecto.
- Definir control de acceso y auditoría desde el diseño.
- Documentar el tratamiento de datos y el propósito de cada dato.

---

[← Volver al README principal](../../README.md)
