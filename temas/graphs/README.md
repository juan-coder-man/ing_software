# Graphs

## Qué son

Un **grafo** es un conjunto de **nodos** (vértices) y **aristas** (edges) que conectan pares de nodos. Puede ser dirigido o no dirigido, ponderado o no. Se representa con listas de adyacencia (por nodo, lista de vecinos) o matriz de adyacencia.

## Conceptos clave

- **Adyacencia:** dos nodos son adyacentes si hay una arista entre ellos. En dirigidos, la arista tiene dirección (u → v).
- **Grado:** número de aristas incidentes (en no dirigidos); en dirigidos, grado de entrada y de salida.
- **Camino:** secuencia de nodos conectados por aristas. **Camino simple:** sin repetir nodos.
- **Ciclo:** camino que empieza y termina en el mismo nodo.
- **Conectividad:** grafo conexo si hay camino entre cualquier par de nodos; en dirigidos, fuertemente conexo si hay camino en ambos sentidos entre cualquier par.
- **BFS:** por niveles; camino más corto en grafos no ponderados.
- **DFS:** en profundidad; detectar ciclos, componentes conexas, orden topológico (en DAGs).
- **DAG:** grafo dirigido acíclico; admite orden topológico.

## Datos clave

| Operación / problema             | Algoritmo típico    | Tiempo         | Notas              |
| -------------------------------- | ------------------- | -------------- | ------------------ |
| Recorrer todos los nodos         | BFS o DFS           | O(V + E)       | V nodos, E aristas |
| Camino más corto (no pond)       | BFS                 | O(V + E)       |                    |
| Camino más corto (pond. no neg.) | Dijkstra            | O((V+E) log V) | Con heap           |
| Orden topológico                 | DFS o Kahn (grados) | O(V + E)       | Solo DAG           |
| Componentes conexas              | DFS/BFS por nodo    | O(V + E)       |                    |
| Árbol de expansión mínima        | Prim, Kruskal       | O(E log V)     | Grafo no dirigido  |

## Ejemplo: representación por listas de adyacencia

```java
// Grafo no dirigido, n nodos (0..n-1)
List<List<Integer>> buildGraph(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
    for (int[] e : edges) {
        graph.get(e[0]).add(e[1]);
        graph.get(e[1]).add(e[0]);
    }
    return graph;
}
```

## Ejemplo: DFS para detectar ciclo en no dirigido

```java
boolean hasCycle(List<List<Integer>> graph) {
    int n = graph.size();
    boolean[] seen = new boolean[n];
    for (int i = 0; i < n; i++) {
        if (!seen[i] && dfsCycle(graph, i, -1, seen)) return true;
    }
    return false;
}
boolean dfsCycle(List<List<Integer>> graph, int u, int parent, boolean[] seen) {
    seen[u] = true;
    for (int v : graph.get(u)) {
        if (!seen[v]) {
            if (dfsCycle(graph, v, u, seen)) return true;
        } else if (v != parent) return true;
    }
    return false;
}
```

## Ejemplo: orden topológico (Kahn, grados de entrada)

```java
List<Integer> topoSort(List<List<Integer>> graph) {
    int n = graph.size();
    int[] inDegree = new int[n];
    for (List<Integer> adj : graph)
        for (int v : adj) inDegree[v]++;
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++)
        if (inDegree[i] == 0) q.offer(i);
    List<Integer> order = new ArrayList<>();
    while (!q.isEmpty()) {
        int u = q.poll();
        order.add(u);
        for (int v : graph.get(u)) {
            inDegree[v]--;
            if (inDegree[v] == 0) q.offer(v);
        }
    }
    return order.size() == n ? order : Collections.emptyList(); // vacío si hay ciclo
}
```

## Patrones frecuentes

- “Camino más corto” sin pesos → BFS.
- “¿Hay ciclo?” / “recorrer todo” → DFS o BFS.
- “Orden de tareas/dependencias” → DAG + orden topológico.
- “Mínima distancia con pesos no negativos” → Dijkstra.
- Matriz como grafo: cada celda es nodo, vecinos arriba/abajo/izq/der.

---

[← Volver al README principal](../../README.md)
