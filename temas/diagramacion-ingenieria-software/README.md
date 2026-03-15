# Reglas de diagramación para ingenieros de software

Los diagramas en ingeniería de software sirven para comunicar diseño, flujos y arquitectura de forma clara y unívoca. Usar reglas y convenciones consistentes evita malentendidos, facilita la revisión y mantiene la documentación útil a lo largo del tiempo.

## Tipos de diagramas y cuándo usarlos

### UML

| Diagrama     | Propósito principal                                    | Cuándo usarlo                                                |
| ------------ | ------------------------------------------------------ | ------------------------------------------------------------ |
| Casos de uso | Interacción entre actores y sistema                    | Requisitos funcionales, alcance, fronteras del sistema       |
| Clases       | Estructura estática: clases, atributos, relaciones     | Modelado de dominio, diseño OO, contratos entre módulos      |
| Secuencia    | Orden temporal de mensajes entre objetos o componentes | Flujos de una operación, integraciones, APIs                 |
| Actividad    | Flujo de trabajo o algoritmo (decisiones, paralelismo) | Procesos de negocio, lógica compleja, flujos de pantalla     |
| Estado       | Estados de una entidad y transiciones entre ellos      | Ciclos de vida (pedido, usuario, sesión), máquinas de estado |
| Componentes  | Partes del sistema y sus dependencias                  | Vista de módulos o servicios de una aplicación               |
| Despliegue   | Nodos de ejecución y artefactos desplegados            | Infraestructura, contenedores, servidores                    |
| Paquetes     | Agrupación lógica de elementos (módulos, namespaces)   | Estructura de alto nivel del proyecto o del dominio          |

### Datos

| Diagrama              | Propósito                         | Cuándo usarlo                                 |
| --------------------- | --------------------------------- | --------------------------------------------- |
| Entidad-relación (ER) | Entidades, atributos y relaciones | Modelo de datos, esquema de BD, normalización |

### Procesos y flujos

| Diagrama | Propósito                          | Cuándo usarlo                                         |
| -------- | ---------------------------------- | ----------------------------------------------------- |
| Flujo    | Pasos, decisiones, inicio/fin      | Algoritmos simples, flujos de usuario, procedimientos |
| BPMN     | Procesos de negocio estandarizados | Procesos que involucran roles, tareas y eventos       |

### Arquitectura

| Tipo                                          | Propósito                          | Cuándo usarlo                                          |
| --------------------------------------------- | ---------------------------------- | ------------------------------------------------------ |
| C4 (contexto, contenedor, componente, código) | Capas de abstracción del sistema   | Documentar arquitectura de software de forma escalable |
| Infraestructura                               | Servidores, redes, servicios cloud | Despliegue, DevOps, dependencias de entorno            |

## Reglas y convenciones por tipo

### Notación estándar

- **UML:** Seguir UML 2.x para que cualquier ingeniero interprete igual los símbolos (rectángulos, flechas, estereotipos, multiplicidades).
- **C4:** Usar la notación C4 (círculos para personas, cajas para sistemas/contenedores/componentes) para no mezclar con UML de clases.
- **ER:** Notación Chen o crow’s foot según el estándar del equipo o de la herramienta (cardinalidades claras: 1:1, 1:N, N:M).

### Nivel de detalle

- Un diagrama debe tener **un propósito claro**: explicar un flujo, una estructura o un contexto, no todo a la vez.
- Evitar sobrecargar: si hay más de 7–10 elementos relevantes en un mismo nivel, valorar dividir en varios diagramas o filtrar lo no esencial.
- Mantener el **mismo nivel de abstracción** en un mismo diagrama (por ejemplo, no mezclar clases de dominio con tablas de BD en el mismo diagrama de clases).

### Nombres y direcciones

- Nombres claros y consistentes (misma convención en todo el documento: PascalCase para clases, verbos para mensajes en secuencia).
- Flujo de lectura coherente: en diagramas de flujo y secuencia, direccionar izquierda–derecha o arriba–abajo según el estándar del equipo y mantenerlo en todos los diagramas similares.
- Incluir una **leyenda o glosario** cuando se usen símbolos o convenciones propias (colores, estilos de línea) para que no dependan de una persona.

## Buenas prácticas generales

- **Un diagrama, una idea:** Evitar mezclar contexto de negocio, diseño técnico e infraestructura en el mismo dibujo.
- **Mantener actualizados:** Los diagramas deben reflejar el código o el proceso actual; si no, indicar la fecha o la versión a la que corresponden o archivarlos.
- **Versionado:** Preferir herramientas que permitan guardar diagramas en el repositorio (Mermaid, PlantUML, draw.io en XML) para revisión en PRs y historial.
- **Evitar mezclar niveles:** No poner en un diagrama de contexto detalles de implementación ni en un diagrama de clases detalles de infraestructura.

## Herramientas

| Herramienta                   | Uso típico                                               |
| ----------------------------- | -------------------------------------------------------- |
| draw.io / Diagrams.net        | Diagramas genéricos, exportación a repositorio           |
| Mermaid                       | Diagramas en Markdown (secuencia, flujo, clases, C4)     |
| PlantUML                      | UML y otros diagramas desde texto, integrable en CI/docs |
| IDE (IntelliJ, VS Code, etc.) | Diagramas de clases y paquetes generados desde código    |

Elegir según si se prioriza edición visual, documentación como código o generación automática desde el código.

---

[← Volver al README principal](../../README.md)
