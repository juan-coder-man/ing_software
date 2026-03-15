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

### Pipeline como código

- Definir el pipeline en el repositorio (YAML en GitHub Actions/GitLab CI, Jenkinsfile en Jenkins) para versionado, revisión en PR y misma fuente de verdad para todo el equipo.
- Evitar configurar pipelines solo en la UI del servidor de CI; cambios en código permiten historial y rollback de la propia pipeline.
- Diseñar jobs idempotentes y re-ejecutables: que volver a lanzar el mismo commit produzca el mismo resultado.

### Fallar rápido

- Orden recomendado: lint → pruebas unitarias → pruebas de integración; las más rápidas y baratas primero para detectar fallos antes.
- Definir timeouts por job para que un test colgado no bloquee el pipeline indefinidamente.
- Paralelizar jobs independientes (varios lenguajes, suites de tests separadas) para reducir tiempo total.
- Usar cache de dependencias (npm, Maven, Docker layers) para acelerar builds sin sacrificar reproducibilidad.

### Artefactos inmutables

- Un mismo artefacto (imagen Docker, JAR, WAR) debe promoverse entre entornos (staging → prod) sin recompilar; "build once, deploy many".
- Versionar con semántico o hash del commit para rastreabilidad y rollback; no usar "latest" en producción.
- No recompilar por entorno: la configuración por entorno (URLs, feature flags) se inyecta en tiempo de ejecución, no en build.

### Secrets fuera del código

- Nunca commitear claves, tokens ni credenciales en el repositorio; no exponerlos en logs ni en mensajes de error.
- Usar variables de entorno del sistema de CI (marcadas como secretas/masked) o gestores de secretos (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).
- Rotar secretos de forma periódica y tener procedimiento para revocarlos si hay fuga.
- Ejemplo de qué no hacer: `API_KEY=abc123` en un archivo de config versionado o en el YAML del pipeline en texto plano.

### Rollback sencillo

- Estrategias que permitan volver atrás sin recompilar: blue/green (dos entornos, cambiar tráfico), canary (porcentaje de tráfico a nueva versión), versionado de artefactos para redeploy de la versión anterior.
- Decidir qué despliegues son automáticos y cuáles requieren aprobación manual (p. ej. producción); documentar el proceso de rollback.

### Tests

- Definir umbral mínimo de cobertura y hacer fallar el pipeline si no se cumple; integrar con SonarQube o similar.
- Evitar tests flaky: tests que a veces pasan y a veces fallan sin cambio de código; aislarlos, corregirlos o desactivarlos temporalmente con seguimiento.
- Usar datos de prueba deterministas y aislados; no depender de orden de ejecución ni de estado externo compartido.

### Seguridad

- Escaneo de dependencias (Snyk, Dependabot, OWASP Dependency-Check) en el pipeline para vulnerabilidades conocidas; bloquear o alertar según política.
- Usar imágenes base fijas y escaneadas para contenedores; actualizar bases de forma controlada.
- Principio de mínimo privilegio en el pipeline: la identidad que ejecuta el pipeline debe tener solo los permisos necesarios (repos, registries, entornos de despliegue).

### Entornos

- Separar claramente entornos (desarrollo, staging, producción); no usar producción para pruebas.
- Aprobaciones manuales para producción si el negocio lo requiere; en otros entornos se puede desplegar automáticamente tras pasar CI.
- Feature flags para activar/desactivar funcionalidad sin redeploy; permiten despliegue continuo con liberación gradual.

### Observabilidad del pipeline

- Logs y métricas del pipeline (duración por etapa, tasa de éxito, tiempo hasta feedback) para detectar cuellos de botella y tendencias.
- Notificaciones en fallo (Slack, email, etc.) para que el equipo reaccione rápido; opcionalmente notificar también en éxito en canales dedicados.
- Reducir el tiempo de feedback: desde commit hasta resultado del pipeline; objetivos típicos por debajo de 10–15 minutos para la rama principal.

## Herramientas típicas

- **GitHub Actions**, **GitLab CI**: integrados al repositorio, configuración en YAML.
- **Jenkins**: servidor autónomo, gran flexibilidad y plugins. Ver [Jenkins](../jenkins/README.md) en este repositorio.
- **Tekton / OpenShift Pipelines**: pipelines basados en Kubernetes.

---

[← Volver al README principal](../../README.md)
