# GoF: Gang of Four y patrones de diseño

El libro _Design Patterns: Elements of Reusable Object-Oriented Software_ (Gamma, Helm, Johnson, Vlissides — 1994) se conoce como **Gang of Four (GoF)** y recopila 23 patrones de diseño clásicos en tres categorías: creacionales, estructurales y de comportamiento. Son la base de muchos de los patrones que se usan hoy en día.

## Relación con los patrones de diseño

En este repositorio el tema [Patrones de Diseño](../patrones-diseno/README.md) desarrolla patrones con ejemplos y código. El **GoF** es el catálogo original que los nombra y los agrupa; los patrones que allí aparecen (Singleton, Factory, Observer, Strategy, etc.) son los del libro.

## Las tres categorías GoF

| Categoría          | Propósito                                     | Algunos patrones                                                |
| ------------------ | --------------------------------------------- | --------------------------------------------------------------- |
| **Creacionales**   | Creación de objetos desacoplada y flexible    | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| **Estructurales**  | Composición de clases y objetos               | Adapter, Decorator, Facade, Proxy, Composite, Bridge            |
| **Comportamiento** | Interacción y responsabilidades entre objetos | Observer, Strategy, Command, Template Method, State, Iterator   |

## Por qué siguen siendo relevantes

- Dan **nombres comunes** a soluciones recurrentes (Observer, Strategy, etc.), lo que facilita la comunicación.
- Describen el **problema**, la **solución** y las **consecuencias**, no solo el código.
- Son independientes del lenguaje; se adaptan a Java, C#, TypeScript, Go, etc.
- Muchos frameworks y librerías los usan (eventos → Observer, plugins → Strategy, middleware → Decorator/Chain).

Para ver cada patrón con ejemplos y tablas resumidas, consulta [Patrones de Diseño](../patrones-diseno/README.md).

---

[← Volver al README principal](../../README.md)
