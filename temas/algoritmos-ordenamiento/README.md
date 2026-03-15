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

## Selection Sort

En cada pasada se busca el mínimo en el resto del array (desde la posición actual hasta el final) y se intercambia con el elemento en la posición actual. Así la parte izquierda queda ordenada progresivamente. No es estable: puede cambiar el orden relativo de elementos iguales.

**Ejemplo (código):**

```java
void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) minIdx = j;
        }
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

**Uso típico:** didáctico; en la práctica se prefieren otros por su O(n²) siempre.

---

## Insertion Sort

Se mantiene un subarray izquierdo ya ordenado. En cada paso se toma el siguiente elemento y se inserta en su posición correcta dentro de ese subarray, desplazando los mayores. Estable y muy eficiente cuando los datos están casi ordenados (pocas comparaciones e intercambios).

**Ejemplo (código):**

```java
void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int valor = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > valor) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = valor;
    }
}
```

**Uso típico:** n pequeño o secuencias casi ordenadas; a veces usado como paso final en híbridos (p. ej. en Timsort).

---

## Merge Sort

Divide la secuencia en dos mitades, ordena cada mitad recursivamente (Merge Sort) y luego fusiona las dos mitades ya ordenadas en una sola secuencia ordenada. Estable y O(n log n) siempre; requiere espacio O(n) para el array auxiliar de la fusión.

**Idea:** `merge` recibe dos mitades ordenadas y un array auxiliar; va eligiendo el menor de los dos frentes y lo escribe en la salida. La recursión termina cuando el tramo tiene 0 o 1 elemento.

**Ejemplo (código):**

```java
void mergeSort(int[] arr, int izq, int der, int[] aux) {
    if (izq >= der) return;
    int mid = izq + (der - izq) / 2;
    mergeSort(arr, izq, mid, aux);
    mergeSort(arr, mid + 1, der, aux);
    merge(arr, izq, mid, der, aux);
}

void merge(int[] arr, int izq, int mid, int der, int[] aux) {
    for (int k = izq; k <= der; k++) aux[k] = arr[k];
    int i = izq, j = mid + 1;
    for (int k = izq; k <= der; k++) {
        if (i > mid) arr[k] = aux[j++];
        else if (j > der) arr[k] = aux[i++];
        else if (aux[j] < aux[i]) arr[k] = aux[j++];
        else arr[k] = aux[i++];
    }
}
```

Llamada inicial: `mergeSort(arr, 0, arr.length - 1, new int[arr.length])`. El array `aux` es la memoria extra O(n).

**Uso típico:** ordenación estable; listas enlazadas o datos en disco donde Quick Sort no es ideal.

---

## Quick Sort

Elige un pivote (p. ej. el último elemento, el del medio o uno aleatorio para reducir probabilidad de peor caso), particiona el array en "menores o iguales" y "mayores" que el pivote, y aplica recursivamente Quick Sort a cada parte. Rendimiento medio O(n log n); peor caso O(n²) si el pivote es siempre el mínimo o el máximo.

**Idea:** `partition` coloca el pivote en su posición final y devuelve ese índice; luego se ordena `[inicio, p-1]` y `[p+1, fin]`. Elección de pivote: último (simple), medio (mejor con datos ordenados), aleatorio (evita peor caso en promedio).

**Ejemplo (código, pivote último):**

```java
void quickSort(int[] arr, int inicio, int fin) {
    if (inicio >= fin) return;
    int p = partition(arr, inicio, fin);
    quickSort(arr, inicio, p - 1);
    quickSort(arr, p + 1, fin);
}

int partition(int[] arr, int inicio, int fin) {
    int pivote = arr[fin];
    int i = inicio - 1;
    for (int j = inicio; j < fin; j++) {
        if (arr[j] <= pivote) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[fin];
    arr[fin] = temp;
    return i + 1;
}
```

Llamada inicial: `quickSort(arr, 0, arr.length - 1)`.

**Uso típico:** uso general en memoria; muchas librerías estándar lo usan por defecto (a veces con variantes híbridas).

---

## Heap Sort

Se construye un heap máximo (el padre es mayor o igual que los hijos). La raíz es el máximo global; se intercambia con el último elemento del array, se reduce el tamaño "lógico" del heap y se re-heapifica hacia abajo. Se repite hasta que todo el array quede ordenado. No es estable; no requiere memoria adicional aparte del propio array (O(1)).

**Idea:** Usar el array como heap implícito: el hijo izquierdo de `i` está en `2*i+1`, el derecho en `2*i+2`. Fase 1: construir heap (heapify desde la mitad hacia 0). Fase 2: extraer máximo (intercambiar raíz con último), heapify hacia abajo, repetir.

**Ejemplo (código):**

```java
void heapSort(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    for (int tam = n - 1; tam > 0; tam--) {
        int temp = arr[0];
        arr[0] = arr[tam];
        arr[tam] = temp;
        heapify(arr, tam, 0);
    }
}

void heapify(int[] arr, int tam, int i) {
    int mayor = i, izq = 2 * i + 1, der = 2 * i + 2;
    if (izq < tam && arr[izq] > arr[mayor]) mayor = izq;
    if (der < tam && arr[der] > arr[mayor]) mayor = der;
    if (mayor != i) {
        int temp = arr[i];
        arr[i] = arr[mayor];
        arr[mayor] = temp;
        heapify(arr, tam, mayor);
    }
}
```

**Uso típico:** cuando no se puede usar memoria extra O(n) y se quiere O(n log n) garantizado (p. ej. sistemas embebidos o restricciones de RAM).

---

## Cuándo usar qué

- **En la práctica:** usar la implementación de la librería estándar (p. ej. `Arrays.sort` en Java usa variantes de Merge Sort y Quick Sort según el tipo y tamaño).
- **Estabilidad necesaria:** Merge Sort o algoritmos estables de la API.
- **Memoria muy limitada:** Heap Sort o Insertion Sort para n pequeño.
- **Didáctica:** Bubble, Selection e Insertion para entender comparaciones e intercambios.

---

[← Volver al README principal](../../README.md)
