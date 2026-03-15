# Search

30% of companies test this subject.

## Qué son

**Búsqueda** aquí se refiere a localizar un elemento o una posición en una estructura (array, árbol, grafo). Incluye búsqueda binaria en secuencias ordenadas, búsqueda en árboles (BST) y recorridos en grafos (BFS, DFS) cuando el objetivo es encontrar un nodo o un camino.

## Conceptos clave

- **Búsqueda binaria:** en un array ordenado, comparar con el elemento central y descartar la mitad; O(log n) comparaciones.
- **Variantes:** encontrar primera/última aparición, punto de inserción, búsqueda en array rotado o en matriz ordenada por filas/columnas.
- **BFS (amplitud):** nivel a nivel; encuentra el camino más corto en grafos no ponderados.
- **DFS (profundidad):** hasta el fondo y retroceso; útil para recorrer todo el grafo, ciclos, componentes conexas, backtracking implícito.
- **Espacio de búsqueda:** en problemas de optimización, a veces el “search” es sobre un rango numérico (respuesta binaria) o sobre estados (BFS/DFS).

## Datos clave

| Escenario                     | Método típico     | Tiempo       | Espacio  |
| ----------------------------- | ----------------- | ------------ | -------- |
| Array ordenado, 1 valor       | Búsqueda binaria  | O(log n)     | O(1)     |
| Árbol BST                     | Comparar con raíz | O(h)         | O(h) rec |
| Grafo, camino más corto       | BFS               | O(V + E)     | O(V)     |
| Grafo, recorrer todo          | DFS/BFS           | O(V + E)     | O(V)     |
| Búsqueda en rango (respuesta) | Binary search ans | O(log R · f) | O(1)     |

R = rango de la respuesta, f = coste de evaluar un candidato.

## Ejemplo: búsqueda binaria clásica

En un array ordenado ascendente, encontrar el índice de `target` o -1 si no existe.

```java
int binarySearch(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

## Ejemplo: lower bound (primera posición >= target)

```java
int lowerBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

## Ejemplo: BFS camino más corto en grafo no ponderado

```java
int shortestPath(List<List<Integer>> graph, int start, int end) {
    Queue<Integer> q = new LinkedList<>();
    boolean[] seen = new boolean[graph.size()];
    int[] dist = new int[graph.size()];
    q.offer(start);
    seen[start] = true;
    while (!q.isEmpty()) {
        int u = q.poll();
        if (u == end) return dist[u];
        for (int v : graph.get(u)) {
            if (!seen[v]) {
                seen[v] = true;
                dist[v] = dist[u] + 1;
                q.offer(v);
            }
        }
    }
    return -1;
}
```

## Patrones frecuentes

- “Array ordenado” → búsqueda binaria o dos punteros.
- “Camino más corto” en grafo no ponderado → BFS.
- “¿Alcanzable? / recorrer todos” → BFS o DFS.
- “Minimizar el máximo” o “maximizar el mínimo” → a veces la respuesta se busca con binary search sobre el valor.
