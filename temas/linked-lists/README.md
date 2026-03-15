# Linked Lists

## Qué son

Una **lista enlazada** es una estructura en la que cada nodo guarda un valor y una referencia al siguiente nodo (y opcionalmente al anterior, en listas doblemente enlazadas). No hay acceso por índice en O(1); el recorrido es secuencial. La cabeza (head) es la referencia al primer nodo.

## Conceptos clave

- **Nodo:** valor + puntero/referencia `next` (y a veces `prev`).
- **Ventaja frente a array:** inserción y borrado en una posición conocida son O(1) si se tiene la referencia al nodo anterior (o al nodo a borrar con doble enlace).
- **Desventaja:** no hay acceso aleatorio; buscar por índice o por valor es O(n).
- **Dummy head:** nodo ficticio delante del primer elemento para simplificar inserciones/borrados en la cabeza.
- **Dos punteros (rápido y lento):** para encontrar el centro, detectar ciclo, o el n-ésimo nodo desde el final (avanzar uno n pasos y luego ambos hasta que el rápido llegue al final).
- **Reversión:** iterativa con tres punteros (prev, curr, next) o recursiva.

## Datos clave

| Operación                      | Lista enlazada | Array (no ordenado) |
| ------------------------------ | -------------- | ------------------- |
| Acceso por índice              | O(n)           | O(1)                |
| Búsqueda por valor             | O(n)           | O(n)                |
| Inserción al inicio            | O(1)           | O(n) si desplazar   |
| Inserción en posición conocida | O(1)\*         | O(n)                |
| Borrado en posición conocida   | O(1)\*         | O(n)                |

\* Si se tiene la referencia al nodo (o al anterior).

## Ejemplo: definición de nodo (singly linked)

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

## Ejemplo: reversión iterativa

```java
ListNode reverse(ListNode head) {
    ListNode prev = null;
    while (head != null) {
        ListNode next = head.next;
        head.next = prev;
        prev = head;
        head = next;
    }
    return prev;
}
```

## Ejemplo: detectar ciclo (Floyd)

Si un puntero avanza 1 y otro 2 por paso, se encuentran dentro del ciclo si existe; luego se puede encontrar la entrada al ciclo reiniciando uno en head y avanzando ambos a paso 1 hasta que coincidan.

```java
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

## Ejemplo: n-ésimo nodo desde el final

Avanza `fast` n pasos; luego avanza `slow` y `fast` a la vez hasta que `fast` llegue al final; `slow` queda en el n-ésimo desde el final.

```java
ListNode nthFromEnd(ListNode head, int n) {
    ListNode slow = head, fast = head;
    for (int i = 0; i < n; i++) fast = fast.next;
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
}
```

## Patrones frecuentes

- Dummy head para no tratar la cabeza como caso especial.
- Dos punteros (rápido/lento) para centro, ciclo, n-ésimo desde el final.
- Reversión de una porción (por ejemplo, invertir cada k nodos).
- Unir dos listas ordenadas: comparar cabezas y enlazar la menor; útil para merge sort de listas.

---

[← Volver al README principal](../../README.md)
