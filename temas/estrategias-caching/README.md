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

## Implementación técnica

### En memoria (en la aplicación)

Se usa una estructura de datos (p. ej. `Map`) donde la clave es el identificador del recurso y el valor el dato o resultado cacheado. Para TTL se guarda un timestamp de expiración y se comprueba antes de devolver el valor; si expiró, se elimina y se vuelve a pedir al origen. En entornos con límite de memoria puede usarse una caché LRU (Least Recently Used) que expulsa los ítems menos usados. En Flutter, para archivos e imágenes suele usarse un paquete como **flutter_cache_manager**, que gestiona directorio, TTL e invalidación.

Ejemplo mínimo de cache-aside en memoria con TTL (pseudocódigo):

```
estructura CacheEntry { valor, expiraEn }
cache = Map<clave, CacheEntry>

función obtener(clave):
  si cache[clave] existe y ahora < cache[clave].expiraEn:
    devolver cache[clave].valor
  dato = origen.leer(clave)
  cache[clave] = CacheEntry(dato, ahora + TTL)
  devolver dato
```

### Con Redis (o similar)

Se usa un cliente Redis (p. ej. en Node, Java o desde el backend) para almacenar pares clave-valor. **SET** guarda el valor; **EXPIRE** fija el TTL en segundos. **GET** devuelve el valor o nil si no existe o expiró. La invalidación se hace con **DEL** (una clave) o con comandos por patrón (p. ej. **SCAN** + **DEL** o **UNLINK**). La estrategia cache-aside se implementa: intentar GET; si falla, leer del origen, SET y EXPIRE.

### Cabeceras HTTP

El servidor puede indicar cómo cachear con **Cache-Control** (p. ej. `max-age=300`, `no-cache`, `no-store`). **ETag** y **Last-Modified** permiten validación: el cliente envía **If-None-Match** (ETag) o **If-Modified-Since**; si el recurso no cambió, el servidor responde **304 Not Modified** sin cuerpo y el cliente reutiliza su copia local. Navegadores y clientes como `http` o **dio** en Flutter pueden usar estas cabeceras automáticamente cuando el servidor las envía correctamente.

---

[← Volver al README principal](../../README.md)
