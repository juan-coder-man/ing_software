# Greedy Algorithms

31% of companies test this subject.

## Qué son

Un algoritmo **greedy** (voraz) toma en cada paso la decisión que parece mejor localmente, sin reconsiderar decisiones anteriores. No siempre lleva al óptimo global; cuando sí, suele haber una estructura (subestructura óptima + elección greedy) que lo justifica.

## Conceptos clave

- **Elección greedy:** criterio local (por ejemplo: “elegir el que termina antes”, “el de mayor valor por unidad de peso”).
- **Subestructura óptima:** un óptimo del problema contiene óptimos de subproblemas (por ejemplo, en intervalos: si se elige un intervalo, el resto del problema es un subproblema del mismo tipo).
- **Demostración:** para estar seguros de que el greedy es correcto, suele hacerse por intercambio (cualquier otra solución puede transformarse en la greedy sin empeorar) o por inducción.
- **Ordenación previa:** muchos greedy requieren ordenar por un criterio (fin de intervalo, ratio valor/peso, etc.) y luego recorrer en ese orden.

## Datos clave

| Problema típico         | Criterio greedy típico           | Complejidad                           |
| ----------------------- | -------------------------------- | ------------------------------------- |
| Intervalos no solapados | Elegir por **fin** más temprano  | O(n log n)                            |
| Moochila fraccional     | Mayor valor/peso primero         | O(n log n)                            |
| Código Huffman          | Unir los dos de menor frecuencia | O(n log n)                            |
| Monedas (cambio)        | Mayor moneda que quepa           | O(n) si hay denominaciones “normales” |

No todos los problemas tienen solución greedy óptima; a veces el greedy es heurística (aproximación).

## Ejemplo: intervalos no solapados (máximo número)

Dados intervalos [inicio, fin], elegir el máximo número de intervalos que no se solapan. Criterio: ordenar por **fin** ascendente y elegir siempre el siguiente que empiece después del último fin elegido.

```java
int maxNonOverlapping(int[][] intervals) {
    Arrays.sort(intervals, Comparator.comparingInt(a -> a[1]));
    int count = 1;
    int lastEnd = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= lastEnd) {
            count++;
            lastEnd = intervals[i][1];
        }
    }
    return count;
}
```

## Ejemplo: saltar hasta el final (Jump Game)

Array `nums`: desde la posición `i` puedes saltar hasta `i + nums[i]`. ¿Se puede llegar al último índice? Greedy: mantener el “alcance” máximo alcanzable; si en algún paso el índice actual supera el alcance, es imposible.

```java
boolean canJump(int[] nums) {
    int reach = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i > reach) return false;
        reach = Math.max(reach, i + nums[i]);
    }
    return true;
}
```

## Patrones frecuentes

- Ordenar y luego recorrer una vez.
- Mantener un “estado” (último fin, alcance máximo, suma actual) y actualizarlo con la elección greedy.
- Si el enunciado pide “máximo número”, “mínimo de algo” o “¿se puede?”, considerar si una elección local óptima conduce al global.
