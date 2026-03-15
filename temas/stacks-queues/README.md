# Stacks and Queues

## Qué son

- **Pila (stack):** estructura LIFO (Last In, First Out). Operaciones: push (añadir al tope), pop (quitar del tope), peek/top (ver el tope). Útil para paréntesis equilibrados, inversión, DFS iterativo, evaluación de expresiones.
- **Cola (queue):** estructura FIFO (First In, First Out). Operaciones: enqueue (añadir al final), dequeue (quitar del frente), peek (ver el frente). Útil para BFS, nivel a nivel, buffers.

## Conceptos clave

- **Stack:** un solo extremo activo (tope). Implementación con array o lista enlazada; push/pop O(1) amortizado.
- **Queue:** dos extremos (frente y final). Implementación con array circular o con dos listas; enqueue/dequeue O(1) amortizado.
- **Deque (double-ended queue):** inserción y borrado por ambos extremos; sirve para colas con prioridad por extremos o ventanas deslizantes con máximo/mínimo (monotonic deque).
- **Cola de prioridad (heap):** no es FIFO; cada elemento tiene prioridad; salen primero los de mayor (o menor) prioridad. Útil para “el mínimo/máximo actual”, merge de k listas, Dijkstra.

## Datos clave

| Estructura    | Push/Enqueue | Pop/Dequeue | Peek | Uso típico                 |
| ------------- | ------------ | ----------- | ---- | -------------------------- |
| Stack         | O(1)         | O(1)        | O(1) | Paréntesis, DFS, reversión |
| Queue         | O(1)         | O(1)        | O(1) | BFS, buffers               |
| Deque         | O(1)         | O(1)        | O(1) | Ventana deslizante, BFS    |
| PriorityQueue | O(log n)     | O(log n)    | O(1) | K-ésimo, Dijkstra, merge   |

## Ejemplo: paréntesis equilibrados

```java
boolean isBalanced(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') stack.push(c);
        else {
            if (stack.isEmpty()) return false;
            char open = stack.pop();
            if ((c == ')' && open != '(') || (c == ']' && open != '[') || (c == '}' && open != '{'))
                return false;
        }
    }
    return stack.isEmpty();
}
```

## Ejemplo: cola con dos pilas

Implementar una cola usando solo dos pilas: una para “entrada” y otra para “salida”. Al hacer dequeue, si la pila de salida está vacía, volcar toda la pila de entrada en la de salida (invirtiendo el orden).

```java
class QueueWithStacks {
    Deque<Integer> in = new ArrayDeque<>();
    Deque<Integer> out = new ArrayDeque<>();

    void enqueue(int x) { in.push(x); }

    int dequeue() {
        if (out.isEmpty())
            while (!in.isEmpty()) out.push(in.pop());
        return out.pop();
    }
}
```

## Ejemplo: BFS con cola

Recorrer un grafo por niveles (por ejemplo, distancia desde un nodo origen).

```java
void bfs(List<List<Integer>> graph, int start) {
    Queue<Integer> q = new LinkedList<>();
    boolean[] seen = new boolean[graph.size()];
    q.offer(start);
    seen[start] = true;
    while (!q.isEmpty()) {
        int u = q.poll();
        for (int v : graph.get(u)) {
            if (!seen[v]) {
                seen[v] = true;
                q.offer(v);
            }
        }
    }
}
```

## Patrones frecuentes

- “Último que abrió, primero que cierra” (paréntesis, etiquetas) → stack.
- “Procesar en orden de llegada” o “nivel a nivel” → queue.
- “Máximo/mínimo en ventana deslizante” → deque monótona.
- “Siempre el menor/mayor actual” → priority queue (heap).
