# Servicios Firebase y Google Cloud

Firebase y Google Cloud ofrecen backend listo para usar (auth, base de datos, almacenamiento) y servicios de infraestructura escalable. Firebase se centra en apps móviles y web con poco backend propio; Google Cloud cubre cargas más complejas y empresariales.

## Firebase: servicios principales

| Servicio              | Propósito                                 | Uso típico                                  |
| --------------------- | ----------------------------------------- | ------------------------------------------- |
| **Authentication**    | Login con email, redes sociales, teléfono | Identidad en apps móviles y web             |
| **Firestore**         | Base de datos NoSQL en tiempo real        | Datos sincronizados, reglas de seguridad    |
| **Realtime Database** | Base de datos JSON en tiempo real         | Sincronización simple, baja latencia        |
| **Storage**           | Almacenamiento de archivos                | Imágenes, vídeos, documentos de usuario     |
| **Cloud Functions**   | Código serverless (Node.js)               | Lógica en backend, webhooks, notificaciones |
| **Hosting**           | Hospedaje estático y SSR                  | SPA, sitios estáticos, previews             |

## Google Cloud (contexto apps)

| Servicio        | Propósito                          | Cuándo usarlo                                                |
| --------------- | ---------------------------------- | ------------------------------------------------------------ |
| **Cloud Run**   | Contenedores serverless            | APIs o workers en Docker sin gestionar servidores            |
| **Cloud Build** | CI/CD y builds de contenedores     | Pipelines que construyen y despliegan en GCP                 |
| **BigQuery**    | Almacenamiento y análisis de datos | Analytics, data warehouse, consultas sobre grandes volúmenes |

Firebase se integra con Google Cloud (por ejemplo, Cloud Functions de Firebase pueden usar servicios GCP). Para apps rápidas con auth y base de datos en tiempo real, Firebase suele ser suficiente; para cargas más pesadas o multi-nube, se combina con Cloud Run, BigQuery u otros servicios GCP.

---

[← Volver al README principal](../../README.md)
