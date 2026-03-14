# CI/CD en DevOps

La integración continua (CI) y el despliegue continuo (CD) automatizan la construcción, las pruebas y el despliegue del software cada vez que hay cambios en el código. Forman parte de las prácticas DevOps para entregar valor con más frecuencia y menor riesgo.

## Conceptos

| Término      | Descripción                                                                               |
| ------------ | ----------------------------------------------------------------------------------------- |
| **CI**       | Cada commit dispara un build y pruebas automáticas; el código se integra a diario.        |
| **CD**       | Los artefactos validados se despliegan de forma automática (o con aprobación) a entornos. |
| **Pipeline** | Secuencia de etapas: checkout → build → test → (análisis) → deploy.                       |

## Etapas típicas de un pipeline

| Etapa        | Objetivo                                 | Herramientas habituales                |
| ------------ | ---------------------------------------- | -------------------------------------- |
| **Build**    | Compilar/empaquetar el código            | Maven, Gradle, npm, dotnet             |
| **Test**     | Ejecutar pruebas unitarias e integración | JUnit, Jest, pytest, Testcontainers    |
| **Análisis** | Linters, cobertura, seguridad estática   | SonarQube, ESLint, Snyk                |
| **Deploy**   | Publicar en entorno (staging/prod)       | Docker, Kubernetes, Terraform, scripts |

## Mejores prácticas

- **Pipeline como código:** Definir el pipeline en el repositorio (YAML, Jenkinsfile) para versionado y revisión.
- **Fallar rápido:** Ejecutar las pruebas más rápidas primero; paralelizar cuando sea posible.
- **Artefactos inmutables:** Un mismo artefacto (imagen, JAR) debe promoverse entre entornos sin recompilar.
- **Secrets fuera del código:** Usar variables de entorno o gestores de secretos (Vault, AWS Secrets Manager), nunca en el repo.
- **Rollback sencillo:** Estrategias de despliegue que permitan volver atrás (blue/green, canary, versionado).

## Herramientas típicas

- **GitHub Actions**, **GitLab CI**: integrados al repositorio, configuración en YAML.
- **Jenkins**: servidor autónomo, gran flexibilidad y plugins. Ver [Jenkins](../jenkins/README.md) en este repositorio.
- **Tekton / OpenShift Pipelines**: pipelines basados en Kubernetes.

---

[← Volver al README principal](../../README.md)
