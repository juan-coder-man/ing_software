# Trees

## Qué son

Un **árbol** es un grafo conexo y acíclico. En entrevistas suele usarse el **árbol binario**: cada nodo tiene como máximo dos hijos (izquierdo y derecho). Variantes: árbol binario de búsqueda (BST), árbol balanceado (AVL, Red-Black), árbol completo o perfecto por niveles.

## Conceptos clave

- **Raíz, hoja, nodo interno:** la raíz no tiene padre; las hojas no tienen hijos; el resto son nodos internos.
- **Altura:** longitud del camino más largo desde la raíz hasta una hoja (a veces se define por aristas, a veces por nodos; conviene aclarar). Árbol vacío: altura -1 o 0 según convención.
- **Nivel/depth:** distancia (en aristas) desde la raíz hasta el nodo.
- **BST (Binary Search Tree):** para cada nodo, todos los de la izquierda son menores y todos los de la derecha son mayores. Recorrido **inorden** en BST da la secuencia ordenada.
- **Recorridos:** preorden (raíz, izquierda, derecha), inorden (izquierda, raíz, derecha), postorden (izquierda, derecha, raíz). Por niveles: BFS.
- **Recursión:** la mayoría de problemas de árboles se expresan bien con “resolver en el subárbol izquierdo, en el derecho, y combinar con la raíz”.

## Datos clave

| Operación en BST   | Promedio | Peor (árbol degenerado) |
| ------------------ | -------- | ----------------------- |
| Búsqueda           | O(log n) | O(n)                    |
| Inserción          | O(log n) | O(n)                    |
| Borrado            | O(log n) | O(n)                    |
| Recorrido completo | O(n)     | O(n)                    |

Árbol balanceado: altura O(log n), por tanto búsqueda/inserción/borrado O(log n) en el peor caso.

## Ejemplo: definición de nodo y recorrido inorden

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}

List<Integer> inorder(TreeNode node) {
    List<Integer> out = new ArrayList<>();
    if (node == null) return out;
    out.addAll(inorder(node.left));
    out.add(node.val);
    out.addAll(inorder(node.right));
    return out;
}
```

## Ejemplo: altura del árbol

```java
int height(TreeNode node) {
    if (node == null) return -1; // por aristas; usar 0 si se cuenta por nodos
    return 1 + Math.max(height(node.left), height(node.right));
}
```

## Ejemplo: ¿es BST válido?

Mantener el rango [min, max] permitido para el nodo actual; actualizar el rango al bajar a izquierda (max = node.val) o derecha (min = node.val).

```java
boolean isValidBST(TreeNode node) {
    return isValidBST(node, Long.MIN_VALUE, Long.MAX_VALUE);
}
boolean isValidBST(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max);
}
```

## Patrones frecuentes

- Definir recursión: “qué devuelve la función para un subárbol” y combinar con la raíz.
- ¿Simétrico? ¿Balanceado? ¿BST? → recorrido y condiciones por nodo.
- Camino máximo/suma: en cada nodo considerar “mejor camino que termina aquí” (incluyendo o no la raíz).
- “Por niveles” → BFS con cola; a veces guardar nivel para agrupar.
