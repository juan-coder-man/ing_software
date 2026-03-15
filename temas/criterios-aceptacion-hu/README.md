# Criterios de aceptación funcionales y no funcionales, y historias de usuario

Los criterios de aceptación definen las condiciones que debe cumplir un ítem para considerarse terminado. Pueden ser funcionales (qué hace el sistema) o no funcionales (cómo debe comportarse en términos de calidad). Las historias de usuario (HU) se construyen a partir de estos criterios para que el equipo y el negocio tengan una definición clara de “hecho”.

## Criterios de aceptación no funcionales

Son condiciones que definen **cómo** debe comportarse el sistema: rendimiento, seguridad, usabilidad, disponibilidad, escalabilidad, accesibilidad, etc. No describen una acción concreta del usuario ni una regla de negocio, sino atributos de calidad medibles.

| Tipo (ejemplo) | Descripción breve                                  |
| -------------- | -------------------------------------------------- |
| Rendimiento    | Tiempos de respuesta, throughput, concurrencia     |
| Seguridad      | Autenticación, autorización, cifrado, cumplimiento |
| Usabilidad     | Claridad de la interfaz, flujos intuitivos         |
| Disponibilidad | Uptime, recuperación ante fallos                   |
| Escalabilidad  | Capacidad de crecer en carga o datos               |
| Accesibilidad  | Cumplimiento de estándares (p. ej. WCAG)           |

### Diferencia con los criterios funcionales

- **Funcional:** describe comportamientos o acciones del sistema (qué hace). Ejemplo: “Los usuarios deben autenticarse antes de acceder al sistema.”
- **No funcional:** define atributos de calidad (cómo debe ser). Ejemplo: “El sistema debe cargar la página principal en menos de 3 segundos.”

Si un criterio habla de una **funcionalidad** (autenticarse, guardar, enviar), es funcional. Si habla de una **cualidad** (tiempo, cantidad de usuarios, nivel de accesibilidad), es no funcional.

### Ejemplos de criterios no funcionales

- **Rendimiento:** El sistema debe responder cualquier solicitud en menos de 500 ms bajo carga normal.
- **Concurrencia:** El sistema debe soportar 1.000 usuarios simultáneos.
- **Accesibilidad:** La aplicación debe cumplir con WCAG 2.1 nivel AA.

### Cómo medir criterios no funcionales

- **Rendimiento:** pruebas de carga, percentiles de latencia (p95, p99), throughput (req/s).
- **Seguridad:** pruebas de penetración, escaneos de vulnerabilidades, métricas como intentos fallidos bloqueados, cumplimiento de estándares (OWASP, ISO 27001).
- **Accesibilidad:** auditorías con herramientas y criterios WCAG.

## Cálculos útiles para rendimiento

### Hilos o instancias mínimas

Si un servicio procesa una solicitud en promedio en 200 ms (0,2 s), cada hilo puede manejar 5 solicitudes por segundo. Para 1.000 req/s se necesitan mínimo **200** hilos o instancias concurrentes: 1.000 ÷ 5 = 200.

### Tiempo de respuesta promedio bajo carga

Se calcula sumando todos los tiempos de respuesta y dividiendo entre el número total de solicitudes procesadas.

### Evaluar si el sistema cumple un tiempo máximo

Ejemplo: tiempo máximo 1 s, 200 usuarios concurrentes, cada usuario envía una solicitud cada 5 s.

- Carga: 200 ÷ 5 = 40 req/s.
- Se prueba el sistema con esa carga y se valida que el tiempo máximo se mantenga por debajo de 1 s.

### Herramientas de medición

**JMeter** o **Gatling** permiten simular usuarios concurrentes y medir latencia, throughput y errores bajo carga realista. Son adecuadas para validar criterios de rendimiento y concurrencia.

## Construcción de historias de usuario (HU)

Una historia de usuario suele seguir la forma: “Como [rol], quiero [acción] para [beneficio].” Los criterios de aceptación (funcionales y no funcionales) se escriben en la misma HU o en el ítem del backlog y sirven para:

- Definir cuándo la HU está “terminada”.
- Evitar ambigüedades entre negocio y desarrollo.
- Permitir pruebas y validación objetiva.

Cada criterio debe ser **medible o comprobable**. Los no funcionales se redactan con métricas o estándares concretos (tiempos, niveles WCAG, número de usuarios simultáneos, etc.).

---

[← Volver al README principal](../../README.md)
