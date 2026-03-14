# Mejoras en Java a medida que sube de versión

Las actualizaciones de Java suelen tocar tres frentes: el **lenguaje** (nueva sintaxis y construcciones, como lambdas, records o pattern matching), la **JVM** (rendimiento, garbage collection, y desde Java 21 los virtual threads), y las **APIs de la JDK** (nuevas clases y métodos en la biblioteca estándar, por ejemplo `java.time`, `HttpClient` o las factory methods de colecciones). Las versiones LTS (8, 11, 17, 21) concentran cambios más relevantes y soporte a largo plazo; las no LTS añaden mejoras incrementales y características en preview. Esta guía resume hitos por versión para orientar el repaso y la decisión de actualizar.

## Tabla resumen (hitos por versión)

| Versión   | Año        | Mejoras destacadas                                                                                                     |
| --------- | ---------- | ---------------------------------------------------------------------------------------------------------------------- |
| **8**     | 2014       | Lambdas, Stream API, Optional, interfaces con default/static, Nashorn (JS en JVM), nueva API de fecha/hora (java.time) |
| **9**     | 2017       | Módulos (JPMS), JShell, factory methods para colecciones (List.of, Set.of, Map.of)                                     |
| **10**    | 2018       | Inferencia de tipos locales (var)                                                                                      |
| **11**    | 2018 (LTS) | HttpClient en la JDK, String.isBlank/lines/strip, Optional.isEmpty, ejecutar .java sin compilar explícito              |
| **12–14** | 2019–2020  | Switch expressions (preview → estándar), Text blocks (preview), Records (preview)                                      |
| **15**    | 2020       | Text blocks estándar, Sealed classes (preview)                                                                         |
| **16**    | 2021       | Records, pattern matching for instanceof                                                                               |
| **17**    | 2021 (LTS) | Sealed classes estándar, contexto de serialización más estricto                                                        |
| **21**    | 2023 (LTS) | Virtual threads (project Loom), record patterns, pattern matching for switch estándar, sequenced collections           |

---

## Java 8: lambdas y Streams

Introdujo programación funcional en el ecosistema Java: expresiones lambda, referencias a método e interfaces funcionales. La Stream API permite operaciones encadenadas (filter, map, reduce) sobre colecciones sin mutar estado.

**Ejemplo (código):**

```java
List<String> nombres = List.of("Ana", "Luis", "Eva");
List<String> enMayusculas = nombres.stream()
    .filter(s -> s.length() > 3)
    .map(String::toUpperCase)
    .toList();
```

---

## Java 9: módulos (JPMS)

El sistema de módulos (Java Platform Module System) permite definir dependencias explícitas entre módulos y ocultar paquetes internos. La JDK se modularizó; las aplicaciones pueden usar `module-info.java` para declarar qué exportan y qué requieren.

---

## Java 11 (LTS): HttpClient y mejoras de API

`java.net.http.HttpClient` forma parte de la JDK para peticiones HTTP. Mejoras en `String` (isBlank, lines, strip) y en Optional (isEmpty). Posibilidad de ejecutar un único archivo .java sin compilarlo antes (`java Archivo.java`).

---

## Java 17 (LTS) y 21 (LTS): records, sealed, virtual threads

- **Records:** tipos inmutables para datos (constructor, getters, equals/hashCode generados).
- **Sealed classes/interfaces:** restringen qué clases pueden extender o implementar un tipo.
- **Virtual threads (21):** threads ligeros gestionados por la JVM para concurrencia con menos coste que los platform threads.

**Ejemplo (código):** Record y sealed (simplificado):

```java
public record Usuario(long id, String nombre) {}

public sealed interface Figura permits Circulo, Rectangulo {}
public final class Circulo implements Figura { /* ... */ }
public final class Rectangulo implements Figura { /* ... */ }
```

---

[← Volver al README principal](../../README.md)
