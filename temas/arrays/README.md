# Arrays

## Qué son

Un **array** (arreglo) es una estructura de datos que almacena elementos del mismo tipo en posiciones contiguas de memoria, accesibles por un índice numérico (0-based en la mayoría de lenguajes). Permite acceso directo en O(1) por índice.

## Conceptos clave

- **Tamaño fijo** (en muchos lenguajes) o dinámico (listas/vectores).
- **Índice:** posición del elemento; primer elemento suele ser índice 0.
- **Recorrido:** por índice, o con `for-each` cuando no se necesita la posición.
- **Subarrays:** segmentos contiguos `[i..j]`; útiles para ventanas deslizantes o prefijos/sufijos.
- **Orden:** mantener orden, rotar, invertir, son operaciones muy preguntadas.

## Datos clave

| Operación            | Complejidad típica | Notas                            |
| -------------------- | ------------------ | -------------------------------- |
| Acceso por índice    | O(1)               | Directo por dirección de memoria |
| Búsqueda lineal      | O(n)               | Sin orden: recorrer todo         |
| Búsqueda binaria     | O(log n)           | Solo si el array está ordenado   |
| Inserción al final   | O(1) amortizado    | En estructuras dinámicas         |
| Inserción en medio   | O(n)               | Desplazar elementos              |
| Eliminación en medio | O(n)               | Desplazar elementos              |

## Ejemplo: suma de subarray

Dado un array y dos índices `left` y `right`, devolver la suma de elementos entre ambos (inclusive).

```java
int sumSubarray(int[] arr, int left, int right) {
    int sum = 0;
    for (int i = left; i <= right; i++) {
        sum += arr[i];
    }
    return sum;
}
```

Con **array de prefijos** la suma de cualquier subarray es O(1) tras preprocesar:

```java
// prefijo[i] = arr[0] + arr[1] + ... + arr[i-1]
// suma(left, right) = prefijo[right+1] - prefijo[left]
int sumWithPrefix(int[] prefix, int left, int right) {
    return prefix[right + 1] - prefix[left];
}
```

## Patrones frecuentes en entrevistas

- Dos punteros (inicio/fin o rápido/lento).
- Ventana deslizante (subarray de longitud fija o variable).
- Prefijos y sufijos (sumas, máximos, mínimos).
- Ordenación in-place, rotación, búsqueda en array rotado.

---

[← Volver al README principal](../../README.md)
