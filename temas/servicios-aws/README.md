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

## Buenas prácticas

- Usar **IAM** con permisos mínimos por recurso y evitar claves de root en aplicaciones.
- Separar entornos (dev, staging, prod) con cuentas o VPCs distintas cuando sea posible.
- Aprovechar **regiones y zonas de disponibilidad** para alta disponibilidad y baja latencia.
- Monitorear costes con **Cost Explorer** y alertas de presupuesto; etiquetar recursos para facturación.
- Preferir servicios gestionados (Lambda, RDS, S3) para reducir operación y aumentar resiliencia.

---

[← Volver al README principal](../../README.md)
