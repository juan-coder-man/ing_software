# Sorting

## Qué son

**Ordenar** es reordenar una secuencia de elementos según un criterio (numérico, lexicográfico, etc.). En entrevistas suele usarse la ordenación como paso previo para aplicar búsqueda binaria, dos punteros o eliminar duplicados de forma eficiente.

## Conceptos clave

- **Orden estable:** elementos con la misma clave mantienen su orden relativo (útil cuando se ordena por un campo y se quiere desempate por otro).
- **In-place:** ordenar sin usar espacio extra relevante (O(1) o O(log n) adicional).
- **Comparación:** la mayoría de algoritmos comparan elementos; el límite inferior para ordenación por comparación es O(n log n).
- **Ordenación por conteo/radix:** si el rango de valores es acotado, se puede bajar a O(n) o O(n·k).

## Datos clave

| Algoritmo     | Tiempo promedio | Peor caso  | Estable | Espacio  | Uso típico en entrevistas       |
| ------------- | --------------- | ---------- | ------- | -------- | ------------------------------- |
| Quick Sort    | O(n log n)      | O(n²)      | No      | O(log n) | Por defecto en muchas librerías |
| Merge Sort    | O(n log n)      | O(n log n) | Sí      | O(n)     | Estable, listas enlazadas       |
| Heap Sort     | O(n log n)      | O(n log n) | No      | O(1)     | In-place, sin espacio extra     |
| Tim Sort      | O(n log n)      | O(n log n) | Sí      | O(n)     | Implementación en Java, Python  |
| Counting Sort | O(n + k)        | O(n + k)   | Sí      | O(k)     | Enteros en rango acotado        |

En código: normalmente se usa la función estándar del lenguaje (`Arrays.sort`, `Collections.sort`, etc.) salvo que el enunciado pida un algoritmo concreto o propiedades específicas (estable, in-place).

## Ejemplo: ordenar y usar dos punteros

Comprobar si hay dos elementos que sumen un valor objetivo.

```java
boolean hasTwoSum(int[] arr, int target) {
    Arrays.sort(arr);
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return true;
        if (sum < target) left++;
        else right--;
    }
    return false;
}
```

Ordenar: O(n log n). Dos punteros: O(n). Total: O(n log n).

## Ejemplo: ordenar por criterio compuesto

Ordenar personas por edad y, si tienen la misma edad, por nombre.

```java
// Java: Comparator
Arrays.sort(persons, Comparator
    .comparingInt(Person::getAge)
    .thenComparing(Person::getName));
```

## Patrones frecuentes

- Ordenar para luego hacer búsqueda binaria o dos punteros.
- Ordenar por un atributo para convertir el problema en “barrido” o barrido + estructura auxiliar.
- K-ésimo mayor/menor: QuickSelect O(n) promedio, o ordenar y tomar el índice (n-k).
- Intervalos: ordenar por inicio (o fin) para fusionar o cubrir.
- Anagramas: clave común = cadena ordenada; agrupar con mapa.
