# Buenas prácticas en base de datos

Resumen de prácticas recomendadas para diseño, consultas y operación en bases de datos relacionales, con enfoque en Oracle. El objetivo es mejorar rendimiento, mantenibilidad y consistencia sin redacción en formato pregunta–respuesta.

## Índices: creación y gestión

Los índices son estructuras que permiten localizar filas sin recorrer toda la tabla; su creación y gestión determinan en gran medida el rendimiento de las consultas.

Indexar columnas que participan en condiciones de filtrado, uniones y ordenación o agrupación:

- **WHERE:** columnas usadas en igualdades, rangos o comparaciones que reducen el conjunto de filas.
- **JOIN:** columnas de las claves de unión (foreign keys y sus correspondientes primary keys).
- **ORDER BY y GROUP BY:** columnas que ordenan o agrupan con frecuencia, para evitar ordenaciones en disco.

Conviene priorizar columnas con buena selectividad (valores distintos respecto al total de filas) y evitar índices en columnas con pocos valores distintos salvo que el patrón de consultas lo justifique. Revisar índices no usados y el coste de mantenimiento en escrituras (INSERT/UPDATE/DELETE). En Oracle, tipos como B-tree son el estándar; índices bitmap o de función solo cuando el caso de uso lo requiera.

## Optimización de consultas

La optimización de consultas consiste en escribir y estructurar el SQL para que el motor ejecute menos trabajo y use mejor los recursos (I/O, CPU, índices).

- **Índices:** asegurar que las consultas críticas puedan usar índices existentes; evitar expresiones o funciones sobre columnas indexadas en el WHERE que impidan su uso (por ejemplo `UPPER(col)` sin índice de función).
- **Evitar SELECT \*:** listar solo las columnas necesarias para reducir I/O y ancho de fila, y para que el plan de ejecución sea más estable.
- **EXISTS frente a IN:** en subconsultas que comprueban existencia, `EXISTS` suele ser preferible a `IN` cuando la subconsulta puede devolver muchas filas; el optimizador puede cortar en la primera coincidencia con EXISTS. Con conjuntos pequeños y bien definidos, IN puede ser aceptable.

## Manejo de valores nulos

Un valor nulo indica ausencia de dato; en SQL tiene semántica especial (desconocido, no aplicable) y no se compara con `=` ni con otros valores, por lo que hay que tratarlo de forma explícita.

- Usar **IS NULL** e **IS NOT NULL** en condiciones; las comparaciones con `= NULL` no devuelven filas porque NULL no es igual a ningún valor.
- Para dar un valor por defecto o simplificar expresiones cuando el valor puede ser NULL, usar **COALESCE** (estándar) o **NVL** en Oracle: por ejemplo `COALESCE(campo, valor_por_defecto)`. Útil en SELECT, en condiciones y en agregaciones para tratar NULL de forma explícita.

## Vistas y funciones

Las vistas son consultas guardadas con nombre; las funciones en SQL devuelven un valor o un conjunto. Ambas reutilizan lógica pero pueden afectar el rendimiento si no se diseñan con cuidado.

- Evitar vistas que encapsulen lógica pesada o múltiples joins costosos sin necesidad; cada consulta sobre la vista ejecutará esa lógica y puede degradar el rendimiento.
- Mantener vistas ligeras: preferiblemente proyecciones y filtros simples o joins ya optimizados. Si se necesita lógica compleja, valorar funciones que devuelvan conjuntos, vistas materializadas (con refresco controlado) o capa de aplicación, según el caso.
- Las funciones usadas en SQL (por ejemplo en SELECT o WHERE) deben ser eficientes; las que hacen consultas o lógica pesada por fila pueden provocar ejecuciones N+1 o planes poco eficientes.

## Grandes volúmenes de datos

Grandes volúmenes de datos son tablas o conjuntos que exigen estrategias específicas de carga, índice y partición para que las operaciones sigan siendo viables en tiempo y recursos.

- Para cargas masivas, usar **SQL\*Loader** (o mecanismos equivalentes como External Tables, Data Pump) en lugar de INSERT fila a fila; carga por lotes y opciones como DIRECT PATH reducen redo y bloqueos.
- **Índices:** en cargas masivas suele ser más eficiente cargar primero los datos y crear o reconstruir índices después, para evitar actualizar índices en cada fila. En tablas muy grandes, valorar particionamiento por rango, lista o hash para mantener operaciones y consultas acotadas a particiones.

## Procedimientos almacenados vs consultas SQL directas

Se trata de decidir dónde reside la lógica: en el servidor de base de datos (procedimientos/PL-SQL) o en la aplicación (consultas SQL directas y reglas en el backend).

- **Usar procedimientos almacenados (PL/SQL)** cuando la lógica es intensiva en datos: muchas operaciones sobre el mismo conjunto, bucles que procesan cursores, transformaciones complejas que es más eficiente hacer en el motor. También para encapsular operaciones atómicas (transacciones) y reutilización dentro de la base de datos.
- **Mantener lógica en el backend (aplicación)** las reglas de negocio que orquestan varios sistemas, la validación de dominio que puede cambiar con frecuencia y la lógica que debe ser portable entre bases de datos. Las consultas SQL directas (o preparadas) desde la aplicación son adecuadas para CRUD y consultas de lectura claras; los procedimientos almacenados se reservan para bloques de trabajo que benefician de ejecutarse en el servidor de base de datos.

---

[← Volver al README principal](../../README.md)
