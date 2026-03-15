# Servicios AWS

Amazon Web Services (AWS) es una plataforma de cloud computing que ofrece infraestructura, almacenamiento, cómputo, bases de datos y servicios de aplicación bajo demanda. Conocer los servicios principales ayuda a elegir la arquitectura adecuada para cada proyecto.

## Servicios principales

| Servicio        | Propósito                                     | Caso de uso típico                                        |
| --------------- | --------------------------------------------- | --------------------------------------------------------- |
| **EC2**         | Servidores virtuales configurables            | Apps con estado, cargas variables, control total de OS    |
| **Lambda**      | Ejecución de código sin servidor (serverless) | APIs, procesamiento por eventos, tareas puntuales         |
| **S3**          | Almacenamiento de objetos                     | Archivos estáticos, backups, datos para análisis          |
| **RDS**         | Bases de datos relacionales gestionadas       | MySQL, PostgreSQL, etc. con backups y parches automáticos |
| **API Gateway** | Exposición y gestión de APIs HTTP/WebSocket   | Front para Lambda o servicios HTTP, throttling, auth      |
| **CloudFront**  | CDN y caché global                            | Sitios estáticos, streaming, reducir latencia             |
| **DynamoDB**    | Base de datos NoSQL clave-valor/documento     | Alta escalabilidad, baja latencia, sin servidor           |

## Cuándo usar cada cosa (como dev)

- **EC2**: Cuando necesitas un servidor que tú controlas: API en Node/Java/Python, worker que corre 24/7, algo con estado en memoria. Si tu app es “un proceso que corre en un servidor”, suele ser EC2 (o contenedores encima).
- **Lambda**: Cuando no quieres mantener servidores y tu lógica es por eventos o peticiones puntuales: “cuando llega un request”, “cuando se sube un archivo a S3”, “cada X minutos”. APIs simples, webhooks, procesar colas, jobs programados.
- **S3**: Para guardar archivos: estáticos del front (HTML, JS, CSS, imágenes), backups, logs, artefactos de build, datos para análisis. Cualquier “objeto/binario que subes y bajas por URL”.
- **RDS**: Cuando necesitas base de datos relacional (MySQL, PostgreSQL, etc.) y quieres que AWS gestione backups, parches y alta disponibilidad.
- **API Gateway**: Cuando quieres exponer una API HTTP (REST o WebSocket) con URL pública, throttling, autenticación (API keys, JWT) y enrutar a Lambda o a otros servicios. Es el frontal de tu API.
- **CloudFront**: Cuando quieres que tu contenido (páginas, assets, vídeo) se sirva rápido en todo el mundo y/o cachear respuestas. Típico: sitio estático (S3 + CloudFront), reducir carga al backend.
- **DynamoDB**: Cuando necesitas base de datos NoSQL muy escalable, sin gestionar servidores, con latencia baja y acceso por clave. Bueno para sesiones, caché persistente, datos de alta escritura o modelos clave-valor/documento.

Resumen rápido: “API sin servidor” → Lambda + API Gateway. “Archivos o front estático” → S3 (y CloudFront si quieres CDN). “Base SQL” → RDS. “Base NoSQL gestionada” → DynamoDB. “API con servidor siempre encendido” → EC2 (o contenedores).

## Buenas prácticas

- Usar **IAM** con permisos mínimos por recurso y evitar claves de root en aplicaciones.
- Separar entornos (dev, staging, prod) con cuentas o VPCs distintas cuando sea posible.
- Aprovechar **regiones y zonas de disponibilidad** para alta disponibilidad y baja latencia.
- Monitorear costes con **Cost Explorer** y alertas de presupuesto; etiquetar recursos para facturación.
- Preferir servicios gestionados (Lambda, RDS, S3) para reducir operación y aumentar resiliencia.

---

[← Volver al README principal](../../README.md)
