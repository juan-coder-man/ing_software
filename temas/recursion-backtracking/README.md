# Recursion and Backtracking

## Qué son

- **Recursión:** una función se llama a sí misma con argumentos “más pequeños” hasta llegar a un **caso base** que no hace más llamadas. Permite expresar problemas en términos de subproblemas del mismo tipo.
- **Backtracking:** explorar todas las opciones (o muchas) probando una decisión, avanzando recursivamente y, si no se llega a solución, **deshaciendo** esa decisión (backtrack) para probar otra. Se usa en puzzles, generación de combinaciones/permutaciones, y restricciones (N reinas, Sudoku, subconjuntos que suman un valor).

## Conceptos clave

- **Caso base:** condición que detiene la recursión (por ejemplo: lista vacía, índice fuera de rango, meta alcanzada).
- **Caso recursivo:** reducir el problema (tamaño menor, menos opciones) y combinar el resultado con la solución del subproblema.
- **Backtrack:** después de probar una opción (por ejemplo, colocar una reina en una casilla), se restaura el estado antes de probar la siguiente.
- **Poda:** en backtracking, dejar de explorar una rama si ya se sabe que no puede llevar a una solución válida (o a una mejor que la que ya se tiene).
- **Espacio de búsqueda:** árbol de decisiones; backtracking recorre ese árbol (a menudo en DFS).

## Datos clave

| Concepto             | Descripción breve                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------- |
| Profundidad máxima   | Altura de la pila de llamadas; puede ser O(n).                                            |
| Complejidad temporal | Depende del árbol de recursión (ramas × profundidad).                                     |
| Memoización          | Guardar resultados por “estado” para no repetir trabajo (convierte recursión pura en DP). |
| Cola recursiva       | Algunos lenguajes optimizan la última llamada (TCO).                                      |

Backtracking sin poda puede ser exponencial (p. ej. todas las subconjuntos 2^n); con poda o restricciones fuertes se reduce.

## Ejemplo: factorial recursivo

```java
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

## Ejemplo: generar todos los subconjuntos (backtracking)

```java
void subsets(int[] nums, int i, List<Integer> current, List<List<Integer>> result) {
    if (i == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    current.add(nums[i]);
    subsets(nums, i + 1, current, result);
    current.remove(current.size() - 1);
    subsets(nums, i + 1, current, result);
}
```

Cada elemento: lo incluyes o no; al final de la recursión guardas una copia de `current`.

## Ejemplo: permutaciones (todas las ordenaciones)

```java
void permute(int[] nums, int start, List<List<Integer>> result) {
    if (start == nums.length) {
        result.add(Arrays.stream(nums).boxed().toList());
        return;
    }
    for (int i = start; i < nums.length; i++) {
        swap(nums, start, i);
        permute(nums, start + 1, result);
        swap(nums, start, i);
    }
}
```

Intercambias el elemento en `start` con cada candidato y haces backtrack deshaciendo el swap.

## Ejemplo: N reinas (esquema)

En cada fila, probar colocar la reina en cada columna; comprobar que no hay conflicto con reinas ya colocadas; si es válido, seguir con la siguiente fila; si no hay columna válida, hacer backtrack.

La “decisión” es “reina en (fila, col)”; el “deshacer” es quitar esa reina y probar la siguiente columna.

## Patrones frecuentes

- Definir estado (índice, conjunto de elegidos, restricciones) y transición (incluir/no incluir, elegir siguiente).
- Lista o array mutable para la “solución actual”; al hacer backtrack, quitar el último elemento o restaurar valor.
- Poda: si “lo que queda” no puede cumplir la restricción, no seguir por esa rama.
- Problemas de “todas las soluciones” o “una solución” con restricciones → pensar en backtracking.


---

[← Volver al README principal](../../README.md)
