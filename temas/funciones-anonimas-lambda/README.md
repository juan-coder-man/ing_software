# Funciones Anónimas y Expresiones Lambda

Las **funciones anónimas** y las **expresiones lambda** permiten pasar comportamiento como valor sin definir una clase con nombre. En Java, las lambdas son la forma compacta de implementar interfaces funcionales (una sola método abstracto) y sustituyen en muchos casos a las clases anónimas.

## Tabla resumen

| Concepto           | Descripción                                                       | Uso típico en Java                                           |
| ------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------ |
| Clase anónima      | Clase sin nombre que implementa una interfaz o extiende una clase | Listeners, callbacks, comparadores (antes de lambdas)        |
| Lambda             | Expresión que implementa una interfaz funcional de forma concisa  | Streams, Optional, Runnable, Comparator, callbacks           |
| Interfaz funcional | Interfaz con un solo método abstracto                             | `Runnable`, `Comparator<T>`, `Predicate<T>`, `Function<T,R>` |

### Relación entre ambos

Ambos permiten pasar comportamiento como valor sin definir una clase con nombre. La clase anónima es la forma clásica (pre-Java 8); la lambda es la forma compacta para interfaces funcionales. En la práctica, una lambda sustituye a una clase anónima cuando solo se implementa un método abstracto (callbacks, comparadores, Streams).

---

## Clases anónimas

Antes de Java 8, el comportamiento “inline” se expresaba con clases anónimas: se instancia una clase que implementa una interfaz (o extiende una clase) y se definen los métodos en el mismo lugar. Es verboso pero flexible.

**Ejemplo (código):**

```java
// Sin lambda: clase anónima
Comparator<String> comp = new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.length() - b.length();
    }
};
```

---

## Expresiones lambda

Una lambda es una función sin nombre: lista de parámetros, flecha `->` y cuerpo (expresión o bloque). El compilador infiere el tipo por la interfaz funcional esperada. Solo pueden usarse donde el tipo sea una interfaz funcional.

**Ejemplo (vida real):** Dar una “receta” (qué hacer) en lugar de dar un objeto con nombre que hace esa acción: “ordena por longitud” en vez de “usa esta clase ComparadorPorLongitud”.

**Ejemplo (código):**

```java
// Con lambda
Comparator<String> comp = (a, b) -> a.length() - b.length();

// En streams
list.stream()
    .filter(s -> s.length() > 3)
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

**Referencias a método:** `String::toUpperCase` es equivalente a `s -> s.toUpperCase()` cuando el parámetro se usa solo para llamar a ese método.

---

## Consideraciones

- **Variables efectivamente finales:** una lambda solo puede usar variables del ámbito externo que no se reasignen (final o “effectively final”).
- **`this`:** dentro de una lambda, `this` se refiere a la clase que encierra la lambda, no a la propia lambda (no hay instancia de la lambda).
- **Interfaz funcional:** la lambda debe coincidir con la firma del único método abstracto de la interfaz (`Runnable` → no parámetros, void; `Function<T,R>` → un argumento, retorno R).

---

[← Volver al README principal](../../README.md)
