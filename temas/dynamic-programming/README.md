# Dynamic Programming

27% of companies test this subject.

## Qué son

**Programación dinámica (DP)** resuelve problemas dividiéndolos en subproblemas más pequeños y reutilizando las soluciones de esos subproblemas (evitando recalcular). Requiere **subestructura óptima** (un óptimo del problema incluye óptimos de subproblemas) y **subproblemas superpuestos** (los mismos subproblemas aparecen muchas veces).

## Conceptos clave

- **Estado:** qué información define un subproblema (por ejemplo: índice en el array, capacidad restante, posición en una matriz).
- **Transición:** cómo se calcula un estado a partir de otros (ecuación de recurrencia).
- **Casos base:** estados que se resuelven sin recurrencia (por ejemplo: longitud 0, capacidad 0).
- **Orden de cálculo:** rellenar la tabla de abajo hacia arriba (iterativo) o arriba hacia abajo (memoización en recursión) para que al calcular un estado ya se conozcan los dependientes.
- **Optimización de espacio:** a veces solo hace falta la fila o las dos últimas filas (ej. knapsack 1D).

## Datos clave

| Patrón típico      | Estado típico    | Orden / Estructura     |
| ------------------ | ---------------- | ---------------------- |
| Fibonacci          | dp[i]            | 1D, lineal             |
| Subsecuencia común | dp[i][j]         | 2D, matriz             |
| Moochila 0-1       | dp[i][w]         | 2D, por filas          |
| Camino en grid     | dp[i][j]         | 2D, por filas/columnas |
| Subarray máximo    | 1D con “hasta i” | 1D, Kadane             |

Complejidad: normalmente número de estados × coste de cada transición (a menudo O(1)).

## Ejemplo: Fibonacci con DP

```java
int fib(int n) {
    if (n <= 1) return n;
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}
```

## Ejemplo: máxima suma de subarray (Kadane)

```java
int maxSubarraySum(int[] arr) {
    int maxSum = arr[0];
    int currentSum = arr[0];
    for (int i = 1; i < arr.length; i++) {
        currentSum = Math.max(arr[i], currentSum + arr[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

## Ejemplo: subsecuencia común más larga (LCS)

`dp[i][j]` = longitud de LCS de `a[0..i-1]` y `b[0..j-1]`.

```java
int lcs(String a, String b) {
    int n = a.length(), m = b.length();
    int[][] dp = new int[n + 1][m + 1];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a.charAt(i - 1) == b.charAt(j - 1))
                dp[i][j] = 1 + dp[i - 1][j - 1];
            else
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[n][m];
}
```

## Patrones frecuentes

- “Máximo / mínimo de …” o “cuántas formas de …” con subproblemas que se repiten → plantear estado y transición.
- Empezar con recurrencia recursiva y luego pasar a tabla (o memoización).
- Si el estado es 2D y solo depende de la fila anterior, probar a comprimir a 1D o dos vectores.
