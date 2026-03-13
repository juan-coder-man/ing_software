## ¿Qué es la arquitectura de software?

La arquitectura de software describe la estructura fundamental de un sistema: cómo se organizan sus componentes, cómo se comunican entre sí y qué decisiones de diseño de alto nivel condicionan su evolución. Define **módulos**, **capas**, **interfaces** y **reglas de interacción**, y sirve como guía para el desarrollo, el mantenimiento y la escalabilidad del producto.

Una buena arquitectura ayuda a:

- Separar responsabilidades.
- Facilitar el cambio y la evolución del sistema.
- Mejorar la mantenibilidad y la capacidad de prueba.
- Soportar los requisitos no funcionales (rendimiento, seguridad, disponibilidad, escalabilidad, etc.).

No existe una arquitectura perfecta para todos los casos: la elección depende del contexto del proyecto, del equipo y de las restricciones técnicas y de negocio.

## Resumen de arquitecturas

| Arquitectura        | Ámbito               | Descripción breve                                                                                         |
| ------------------- | -------------------- | --------------------------------------------------------------------------------------------------------- |
| Monolítica          | Backend / Web        | Una única aplicación desplegable con toda la lógica, datos e interfaz.                                    |
| En capas (n-tier)   | Backend / Web        | Sistema organizado en capas (presentación, aplicación, dominio, datos) con dependencias unidireccionales. |
| SOA                 | Backend / Web        | Servicios con contratos formales, a menudo orquestados por un bus (ESB); integración entre sistemas.      |
| Microservicios      | Backend / Web        | Servicios pequeños, independientes y desplegables, alineados con subdominios del negocio.                 |
| Hexagonal           | Backend / Web        | Dominio en el centro; puertos (interfaces) y adaptadores aíslan la lógica de infraestructura y UI.        |
| Orientada a eventos | Backend / Web        | Componentes se comunican por eventos asíncronos (colas/buses); emisores y consumidores desacoplados.      |
| Serverless          | Backend / Web        | Funciones o servicios gestionados por el proveedor; escalado y pago por uso automáticos.                  |
| Nativa              | Móvil (plataforma)   | App específica por plataforma (Android/iOS) con lenguajes y herramientas oficiales.                       |
| Híbrida             | Móvil (plataforma)   | Lógica e interfaz con tecnologías web embebidas en un contenedor nativo.                                  |
| Multiplataforma     | Móvil (plataforma)   | Código compartido entre plataformas con capa de UI nativa o casi nativa.                                  |
| MVC                 | Móvil (presentación) | Modelo, Vista y Controlador; el controlador coordina vista y modelo.                                      |
| MVP                 | Móvil (presentación) | Presenter como intermediario entre vista y modelo; mejora la testabilidad.                                |
| MVVM                | Móvil (presentación) | ViewModel expone estado y eventos; la vista observa y se actualiza (binding/reactivo).                    |
| Clean Architecture  | Móvil (presentación) | Capas concéntricas: dominio, datos, presentación; dependencias hacia el centro.                           |

## Arquitecturas clásicas en backend y aplicaciones web

### Arquitectura monolítica

En una arquitectura monolítica toda la lógica de negocio, la capa de acceso a datos y la interfaz (por ejemplo, vistas server-side) se despliegan como **una única aplicación**. Suele existir un solo artefacto desplegable (por ejemplo, un `.jar`, `.war` o una única imagen de contenedor) que contiene todo el sistema.

- **Ventajas**
  - Simplicidad de desarrollo y despliegue al inicio.
  - Depuración y pruebas más sencillas en sistemas pequeños.
  - Menor complejidad operativa (un solo servicio que monitorizar).
- **Desventajas**
  - A medida que crece, el código se vuelve difícil de entender y mantener.
  - Cualquier cambio requiere desplegar todo el sistema.
  - Escalado poco flexible: se escala todo el monolito, no solo las partes más críticas.
- **Casos de uso típicos**
  - Productos en fase inicial (MVP) con equipo pequeño.
  - Aplicaciones con dominio relativamente simple.
  - Entornos donde la complejidad operativa debe mantenerse al mínimo.

### Arquitectura en capas

La arquitectura en capas (n-tier o multicapa) organiza el sistema en **capas con responsabilidades bien definidas**, por ejemplo:

- Capa de presentación (UI o API).
- Capa de aplicación o servicios.
- Capa de dominio/negocio.
- Capa de infraestructura o datos (repositorios, acceso a bases de datos, integraciones externas).

Cada capa solo puede comunicarse con la inmediatamente inferior (o según las reglas acordadas), lo que aporta estructura y separación de responsabilidades.

- **Ventajas**
  - Organización clara del código por responsabilidad.
  - Facilita la reutilización dentro de la misma capa.
  - Patrón muy conocido, bien soportado por frameworks.
- **Desventajas**
  - Dependencias “hacia abajo” pueden acoplar en exceso a la infraestructura.
  - Puede volverse rígida si se fuerzan demasiadas reglas de acceso entre capas.
  - A veces se crean capas “por obligación” sin una necesidad real.
- **Casos de uso típicos**
  - Aplicaciones empresariales tradicionales.
  - Sistemas con lógica de negocio media/alta donde se quiere claridad estructural.
  - Proyectos donde el equipo ya está acostumbrado a este patrón.

## Arquitecturas modernas en backend y aplicaciones web

### Arquitectura orientada a servicios (SOA)

SOA (Service-Oriented Architecture) propone descomponer el sistema en **servicios** que exponen funcionalidades bien definidas, típicamente mediante contratos formales (por ejemplo, servicios SOAP o APIs bien estandarizadas) y a menudo coordinados por un bus de servicios (ESB).

- **Ventajas**
  - Permite separar responsabilidades por dominios o capacidades de negocio.
  - Facilita la integración entre sistemas heterogéneos (legado, terceros, etc.).
  - Mejora la reutilización de servicios compartidos en la organización.
- **Desventajas**
  - Suele introducir complejidad en la orquestación y el gobierno de servicios.
  - Riesgo de crear “servicios demasiado genéricos” o poco alineados con el dominio.
  - Requiere una buena gobernanza para evitar un ecosistema caótico.
- **Casos de uso típicos**
  - Grandes organizaciones con muchos sistemas que necesitan interoperar.
  - Contextos donde se prioriza la integración y la reutilización transversal.

### Arquitectura de microservicios

La arquitectura de microservicios divide el sistema en **servicios pequeños, independientes y desplegables de forma autónoma**, alineados con subdominios del negocio. Cada microservicio es responsable de una parte concreta del dominio y suele tener su propia base de datos o modelo de persistencia.

- **Ventajas**
  - Alta escalabilidad: se escala solo lo que realmente lo necesita.
  - Equipos autónomos que pueden desplegar y evolucionar sus servicios de forma independiente.
  - Aislamiento de fallos mejor que en un monolito (si se diseña correctamente).
- **Desventajas**
  - Mucha mayor complejidad operativa (monitorización, logging distribuido, tracing, despliegues).
  - Complejidad en la comunicación entre servicios (latencia, resiliencia, consistencia de datos).
  - Riesgo de sobreingeniería para sistemas pequeños o equipos reducidos.
- **Casos de uso típicos**
  - Sistemas grandes con múltiples dominios de negocio.
  - Organizaciones con equipos especializados y maduros en prácticas DevOps.
  - Aplicaciones que requieren alta escalabilidad y despliegues muy frecuentes.

### Arquitectura hexagonal (puertos y adaptadores)

La arquitectura hexagonal propone situar el **dominio en el centro** del sistema y aislarlo de los detalles de infraestructura (bases de datos, frameworks, UI, etc.) mediante puertos (interfaces) y adaptadores (implementaciones concretas).

- **Ventajas**
  - Aísla la lógica de negocio de detalles externos.
  - Facilita las pruebas unitarias y de integración del dominio.
  - Permite cambiar tecnologías externas (por ejemplo, base de datos) con menor impacto en el núcleo.
- **Desventajas**
  - Puede resultar más compleja de entender para equipos sin experiencia en DDD o arquitecturas limpias.
  - Al inicio parece “sobrediseñada” para sistemas muy pequeños.
- **Casos de uso típicos**
  - Sistemas donde el dominio es complejo y crítico.
  - Proyectos que necesitan alta mantenibilidad a largo plazo.

### Arquitectura orientada a eventos

En una arquitectura orientada a eventos, los componentes del sistema se comunican principalmente mediante **eventos** publicados y consumidos de forma asíncrona (por ejemplo, a través de colas o buses de mensajería). Un servicio emite un evento cuando ocurre algo relevante (“pedido_creado”) y otros servicios reaccionan suscribiéndose a esos eventos.

- **Ventajas**
  - Desacopla emisores y receptores (no necesitan conocerse directamente).
  - Favorece la escalabilidad y la resiliencia (los consumidores pueden procesar eventos a su ritmo).
  - Modelo natural para procesos de negocio que ocurren en cadenas de pasos.
- **Desventajas**
  - Mayor complejidad para razonar sobre el flujo completo (sistema distribuido, asíncrono).
  - Dificultad en garantizar consistencia fuerte; se trabaja más con consistencia eventual.
  - Depuración de problemas más compleja (eventos perdidos, duplicados, etc.).
- **Casos de uso típicos**
  - Sistemas con alto volumen de operaciones que pueden procesarse de forma asíncrona.
  - Integraciones entre microservicios donde se quiere minimizar el acoplamiento.

### Arquitectura serverless

Serverless se centra en ejecutar **funciones** o pequeños servicios gestionados por un proveedor cloud, sin necesidad de administrar servidores de forma directa. El desarrollador define funciones que se disparan ante eventos (peticiones HTTP, mensajes, cron, etc.) y el proveedor gestiona el escalado y la infraestructura.

- **Ventajas**
  - Escalado automático y pago por uso.
  - Menor carga operativa (sin gestión directa de servidores).
  - Ideal para cargas variables o esporádicas.
- **Desventajas**
  - Dependencia fuerte del proveedor (vendor lock-in).
  - Limitaciones de tiempo de ejecución, memoria o almacenamiento según la plataforma.
  - No siempre adecuado para procesos de larga duración o con requisitos muy específicos de infraestructura.
- **Casos de uso típicos**
  - APIs ligeras, procesos batch, tareas programadas.
  - Prototipos o funcionalidades que se ejecutan de forma intermitente.

## Arquitecturas para desarrollo móvil

### Arquitecturas de plataforma móvil

#### Nativa

Desarrollo nativo significa construir una aplicación específica para cada plataforma (por ejemplo, Android y iOS) utilizando sus herramientas y lenguajes oficiales.

- **Ventajas**
  - Máximo aprovechamiento de las capacidades de la plataforma (rendimiento, acceso a hardware, UX nativa).
  - Mejor integración con patrones y guías de diseño propios de cada sistema operativo.
- **Desventajas**
  - Dos (o más) bases de código que mantener.
  - Mayor coste de desarrollo y coordinación entre equipos.
- **Casos de uso típicos**
  - Aplicaciones con requisitos de rendimiento muy exigentes (juegos, apps de gráficos intensivos).
  - Productos donde la experiencia de usuario nativa es prioritaria.

#### Híbrida

En las aplicaciones híbridas, gran parte de la lógica y la interfaz se implementan con tecnologías web (HTML, CSS, JavaScript) embebidas en un contenedor nativo.

- **Ventajas**
  - Reutilización de conocimientos y tecnologías web.
  - Una sola base de código para múltiples plataformas.
- **Desventajas**
  - Rendimiento y experiencia de usuario a menudo inferiores a las apps nativas.
  - Limitaciones para acceso a ciertas capacidades avanzadas del dispositivo.
- **Casos de uso típicos**
  - Aplicaciones de contenido donde la UX nativa no es crítica.
  - Proyectos con presupuesto reducido o equipos fuertes en web.

#### Multiplataforma

El enfoque multiplataforma (cross-platform) busca **compartir una parte importante del código** (lógica de negocio, modelos, servicios) entre Android e iOS, manteniendo una capa de interfaz nativa o casi nativa.

- **Ventajas**
  - Reducción del esfuerzo de desarrollo y mantenimiento al compartir lógica.
  - Mejor experiencia de usuario que en muchas soluciones híbridas puras.
- **Desventajas**
  - Aún puede requerir código específico para cada plataforma en ciertos casos.
  - Dependencia de frameworks/plataformas concretas y sus ciclos de vida.
- **Casos de uso típicos**
  - Aplicaciones de negocio con lógica compleja pero UX estándar.
  - Equipos que quieren llegar a varias plataformas con recursos limitados.

### Patrones de arquitectura de presentación en apps móviles

Estos patrones se centran en cómo organizar la lógica de presentación (pantallas, vistas, controladores) y su relación con la lógica de negocio y los datos.

#### MVC (Model–View–Controller)

Separa la aplicación en:

- **Model**: datos y reglas de negocio.
- **View**: representación visual (pantallas, componentes UI).
- **Controller**: coordina las interacciones entre vista y modelo.

- **Ventajas**
  - Patrón sencillo y conocido.
  - Buena separación inicial entre UI y modelo.
- **Desventajas**
  - En apps grandes, los controladores pueden crecer demasiado (“Massive View Controller”).
  - Difícil de probar si la lógica se concentra en el controlador/vista.

#### MVP (Model–View–Presenter)

En MVP, la vista delega la lógica en un **presenter**, que actúa como intermediario entre la vista y el modelo.

- **Ventajas**
  - Mejora la testabilidad: el presenter puede probarse sin la vista real.
  - La vista se mantiene más simple, centrada solo en renderizar datos y manejar eventos de UI.
- **Desventajas**
  - Más clases y “pegamento” que en MVC.
  - Riesgo de presenters demasiado grandes si no se gestionan bien las responsabilidades.

#### MVVM (Model–View–ViewModel)

MVVM introduce un **ViewModel** que expone el estado y eventos que la vista consume (por ejemplo, a través de observables, data binding o flujos reactivos). La vista observa al ViewModel y se actualiza automáticamente.

- **Ventajas**
  - Facilita el data binding y patrones reactivos.
  - Mejora la separación entre lógica de presentación y vista concreta.
  - Muy adecuado para frameworks que soportan binding o programación reactiva.
- **Desventajas**
  - Curva de aprendizaje mayor para algunos equipos.
  - Posible complejidad añadida si se abusa de patrones reactivos sin necesidad.

#### Clean Architecture aplicada a móvil

Clean Architecture organiza la app en capas concéntricas:

- **Capa de dominio**: casos de uso y entidades de negocio (independiente de frameworks).
- **Capa de datos**: repositorios, fuentes de datos locales/remotas.
- **Capa de presentación**: controladores, presenters o viewmodels y vistas.

Las dependencias apuntan siempre hacia el dominio, lo que protege la lógica de negocio de los detalles técnicos.

- **Ventajas**
  - Alta mantenibilidad y testabilidad a largo plazo.
  - Facilita cambios de infraestructura (API, base de datos, frameworks de UI).
- **Desventajas**
  - Mayor esfuerzo inicial de diseño y estructura.
  - Puede ser excesiva para aplicaciones muy pequeñas o de vida corta.

## Comparación y criterios de elección

Al elegir una arquitectura es importante evaluar:

- **Tamaño y complejidad del dominio**: arquitecturas más avanzadas (hexagonal, microservicios, Clean Architecture) tienen más sentido cuando el dominio es complejo.
- **Tamaño y madurez del equipo**: arquitecturas distribuidas (microservicios, orientadas a eventos, serverless) requieren equipos con experiencia en DevOps y sistemas distribuidos.
- **Requisitos no funcionales**: rendimiento, escalabilidad, disponibilidad, resiliencia y seguridad pueden inclinar la balanza hacia ciertos estilos.
- **Plataformas objetivo**: web, backend, móvil nativo, híbrido o multiplataforma.
- **Horizonte temporal del producto**: un MVP puede empezar con una solución más simple (monolito bien estructurado) y evolucionar después.

De forma simplificada:

- Para **productos pequeños o en fase inicial**: monolito bien estructurado, arquitectura en capas y patrones sencillos de presentación (por ejemplo, MVC o MVVM ligeros).
- Para **sistemas empresariales de gran escala**: arquitecturas en capas avanzadas, hexagonales o basadas en microservicios, posiblemente combinadas con orientación a eventos.
- Para **apps móviles**:
  - Pocas plataformas, alta exigencia de UX y rendimiento → nativo con arquitectura de presentación clara (MVVM, MVP, Clean Architecture).
  - Presupuesto limitado, necesidad rápida de cubrir varias plataformas → híbrida o multiplataforma con patrones de presentación que faciliten el testeo y la evolución.

## Conclusión

La arquitectura de software es una herramienta para gestionar la complejidad, no un fin en sí mismo. Cada estilo arquitectónico implica **trade-offs** entre simplicidad, flexibilidad, rendimiento y coste operativo.

La clave está en:

- Entender bien el contexto y los requisitos (funcionales y no funcionales).
- Elegir una arquitectura lo suficientemente simple para el presente, pero que permita evolucionar el sistema.
- Revisar y ajustar las decisiones arquitectónicas a medida que el producto y el equipo crecen.

No se trata de encontrar “la mejor” arquitectura en abstracto, sino la más adecuada para el problema concreto que se quiere resolver.
