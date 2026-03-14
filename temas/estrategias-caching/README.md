# Estrategias de caching

El caching almacena copias de datos o resultados para servir respuestas más rápidas y reducir carga en el origen (API, base de datos, disco). La estrategia elegida determina cuándo se escribe en la caché, cuándo se invalida y cómo se consulte.

## Estrategias principales

| Estrategia                    | Escritura                                                                  | Lectura                                                   | Uso típico                                               |
| ----------------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------- |
| **Cache-aside**               | La aplicación escribe en caché tras leer del origen (lazy).                | Primero caché; si falla, origen y luego se rellena caché. | APIs, BD; control explícito de qué se cachea.            |
| **Write-through**             | Cada escritura va a caché y al origen a la vez.                            | Siempre desde caché (que está al día).                    | Cuando la consistencia lectura es crítica.               |
| **Write-behind (write-back)** | La aplicación escribe en caché; el origen se actualiza de forma asíncrona. | Desde caché.                                              | Alta escritura; se asume cierto retraso en persistencia. |

## TTL e invalidación

- **TTL (Time To Live):** Tiempo máximo que un ítem permanece en caché; tras expirar se considera inválido y se vuelve a pedir al origen.
- **Invalidación explícita:** Ante un evento (actualización en BD, borrado), se elimina o actualiza la entrada en caché para evitar datos obsoletos.
- **Invalidación por versión o tag:** Asociar versiones a conjuntos de datos; al cambiar la versión se descarta la caché afectada.

## Ejemplos de contexto

- **API REST:** Cache-aside con TTL corto para listados; invalidación al crear/editar/borrar.
- **Base de datos:** Cache-aside (p. ej. Redis delante de MySQL); write-through en configuraciones que deban estar siempre al día.
- **CDN:** Caché de recursos estáticos con TTL largo; invalidación por URL o por purge al desplegar.

Elegir la estrategia según patrones de lectura/escritura, requisitos de consistencia y capacidad de invalidar correctamente.

---

[← Volver al README principal](../../README.md)
