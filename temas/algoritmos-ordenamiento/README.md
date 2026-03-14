# Algoritmos de ordenamiento

Los algoritmos de ordenamiento reordenan los elementos de una secuencia según un criterio (por ejemplo orden numérico o lexicográfico). La elección del algoritmo depende del tamaño de los datos, de la memoria disponible y de si se requiere estabilidad.

## Tabla comparativa

| Algoritmo          | Complejidad promedio | Complejidad peor caso | Estable | Memoria adicional | Uso típico                                              |
| ------------------ | -------------------- | --------------------- | ------- | ----------------- | ------------------------------------------------------- |
| **Bubble Sort**    | O(n²)                | O(n²)                 | Sí      | O(1)              | Didáctico, secuencias muy pequeñas                      |
| **Selection Sort** | O(n²)                | O(n²)                 | No      | O(1)              | Didáctico                                               |
| **Insertion Sort** | O(n²)                | O(n²)                 | Sí      | O(1)              | Pequeños n o datos casi ordenados                       |
| **Merge Sort**     | O(n log n)           | O(n log n)            | Sí      | O(n)              | Orden estable, datos en disco                           |
| **Quick Sort**     | O(n log n)           | O(n²)                 | No\*    | O(log n) a O(n)   | Uso general en memoria, por defecto en muchas librerías |
| **Heap Sort**      | O(n log n)           | O(n log n)            | No      | O(1)              | Cuando no se puede usar mucha memoria extra             |

\* Existen variantes de Quick Sort que pueden ser estables con memoria adicional.

**Estable:** elementos con la misma clave mantienen su orden relativo tras ordenar.

---

## Bubble Sort

Recorre la secuencia comparando pares adyacentes e intercambiándolos si están en orden incorrecto. El proceso se repite hasta que no haya más intercambios. Simple pero ineficiente para n grande.

**Ejemplo (código):**

```java
void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

---

## Quick Sort

Elige un pivote (p. ej. el último elemento), particiona el array en “menores o iguales” y “mayores” que el pivote, y aplica recursivamente Quick Sort a cada parte. Muy usado en la práctica; el rendimiento medio es O(n log n).

**Idea:** `partition` coloca el pivote en su posición final y devuelve ese índice; luego se ordena `[inicio, p-1]` y `[p+1, fin]`.

---

## Merge Sort

Divide la secuencia en dos mitades, ordena cada mitad recursivamente (Merge Sort) y luego fusiona las dos mitades ya ordenadas en una sola secuencia ordenada. Estable y O(n log n) siempre; requiere espacio O(n) para la fusión.

**Uso típico:** ordenación estable cuando el tamaño de los datos o el soporte (disco, listas enlazadas) hace preferible no depender del orden de llegada como en Quick Sort.

---

## Cuándo usar qué

- **En la práctica:** usar la implementación de la librería estándar (p. ej. `Arrays.sort` en Java usa variantes de Merge Sort y Quick Sort según el tipo y tamaño).
- **Estabilidad necesaria:** Merge Sort o algoritmos estables de la API.
- **Memoria muy limitada:** Heap Sort o Insertion Sort para n pequeño.
- **Didáctica:** Bubble, Selection e Insertion para entender comparaciones e intercambios.

---

[← Volver al README principal](../../README.md)
