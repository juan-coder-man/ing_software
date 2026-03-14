# Complejidad ciclomática y complejidad anidada

La **complejidad ciclomática** mide la cantidad de caminos independientes en el código; la **complejidad anidada** (anidación) afecta la legibilidad y el mantenimiento. Ambas se usan para detectar funciones o métodos que conviene dividir o simplificar.

## Complejidad ciclomática

- **Definición:** Número de caminos linealmente independientes que puede seguir el flujo (grafo de control). Fórmula habitual: `CC = 1 + número de puntos de decisión` (if, else, switch case, while, for, `&&`, `||`, `?:`, etc.).
- **Uso:** Umbrales típicos por función: 1–10 aceptable, 11–20 revisar, >20 alto riesgo. Herramientas como SonarQube o linters la calculan.
- **Objetivo:** Reducir ramas y decisiones; extraer métodos, usar early return o reemplazar condicionales por polimorfismo/estrategias cuando aplique.

**Ejemplo:** Una función con 4 `if` y un `else` tiene varios caminos; si CC > 10, suele ser buena idea dividirla o simplificar condiciones.

## Complejidad anidada (anidación)

- **Definición:** Nivel de anidación de bloques (if dentro de if, loops dentro de if, etc.). Muchas herramientas cuentan la profundidad máxima (p. ej. "nesting depth").
- **Problema:** Anidación alta (p. ej. > 3–4 niveles) dificulta la lectura y las pruebas; suele indicar que la función hace demasiado.
- **Cómo reducir:** Early return para casos de error o borde; extraer lógica a funciones con nombre claro; evitar anidar más de lo necesario.

**Ejemplo (evitar):**

```java
if (a != null) {
    if (b != null) {
        if (c.isValid()) {
            // lógica muy dentro
        }
    }
}
```

**Mejor:** Salir pronto o extraer: `if (a == null || b == null || !c.isValid()) return;` y luego la lógica principal al mismo nivel.

---

[← Volver al README principal](../../README.md)
