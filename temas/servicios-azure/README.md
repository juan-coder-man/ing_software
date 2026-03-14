# Servicios Azure

Microsoft Azure es una plataforma de cloud que ofrece cómputo, almacenamiento, bases de datos, IA y herramientas de integración empresarial. Muchos servicios son equivalentes o complementarios a AWS y se integran bien con el ecosistema Microsoft.

## Servicios principales

| Servicio            | Propósito                                    | Cuándo usarlo                                           |
| ------------------- | -------------------------------------------- | ------------------------------------------------------- |
| **App Service**     | Hospedaje de aplicaciones web y APIs         | Apps .NET, Node, Java, PHP; escalado automático, slots  |
| **Azure Functions** | Código serverless por eventos o temporizador | Lógica ligera, integración con otros servicios Azure    |
| **Blob Storage**    | Almacenamiento de objetos                    | Archivos, backups, datos para análisis, CDN estático    |
| **Cosmos DB**       | Base de datos NoSQL multi-modelo, global     | Baja latencia global, varios modelos (documento, grafo) |
| **Azure SQL**       | SQL Server gestionado                        | Apps que ya usan SQL Server o requieren T-SQL           |
| **Azure DevOps**    | Repos, pipelines, boards, pruebas            | CI/CD y gestión de trabajo integrada con Azure          |
| **Key Vault**       | Almacenamiento de secretos y certificados    | Claves, connection strings, certificados SSL            |

## Comparativa breve con AWS

| AWS         | Azure equivalente aproximado   |
| ----------- | ------------------------------ |
| EC2         | Virtual Machines               |
| Lambda      | Azure Functions                |
| S3          | Blob Storage                   |
| RDS         | Azure SQL / MySQL / PostgreSQL |
| API Gateway | API Management                 |

Elegir Azure suele tener sentido cuando ya se usan herramientas Microsoft (Active Directory, Office 365, .NET) o cuando el contrato empresarial lo favorece.

---

[← Volver al README principal](../../README.md)
