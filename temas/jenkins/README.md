# Jenkins

Jenkins es un servidor de automatización open source para CI/CD. Permite definir pipelines como código, ejecutar jobs en agentes distribuidos y integrarse con Git, contenedores y herramientas de despliegue mediante plugins.

## Arquitectura

| Concepto                | Descripción                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **Controller (master)** | Orquesta jobs, sirve la UI, no debe usarse para tareas pesadas.                      |
| **Agent (nodo)**        | Ejecuta los pasos del pipeline; puede ser permanente o efímero (Docker, Kubernetes). |

Los agentes se añaden para distribuir carga y ejecutar en distintos entornos (OS, runtimes).

## Tipos de job

| Tipo          | Descripción                                                              |
| ------------- | ------------------------------------------------------------------------ |
| **Freestyle** | Job configurado desde la UI: pasos libres, parámetros, disparadores.     |
| **Pipeline**  | Job definido por un **Jenkinsfile** (declarativo o scripted) en el repo. |

Se recomienda Pipeline + Jenkinsfile para tener el pipeline versionado y revisable.

## Jenkinsfile

**Declarativo (recomendado):**

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
```

**Scripted:** Groovy completo; más flexible y más complejo de mantener.

## Plugins habituales

- **Pipeline**, **Git**: pipelines y clonado de repos.
- **Docker Pipeline**, **Kubernetes**: ejecutar en contenedores o en un clúster K8s.
- **Credentials**: almacenar secretos de forma segura.
- **Blue Ocean**: interfaz moderna para pipelines (opcional).

## Buenas prácticas

- Usar **credenciales** de Jenkins (no claves en el Jenkinsfile); inyectar por variables.
- Ejecutar jobs en **agentes** dedicados, no en el controller.
- Mantener Jenkins y plugins **actualizados**; revisar changelogs y seguridad.
- Preferir **Pipeline declarativo** y compartir lógica con **shared libraries**.
- Limitar permisos con **Matrix / Role-based**; no usar admin para jobs automáticos.

---

[← Volver al README principal](../../README.md)
