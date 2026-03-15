# Dictionaries and Hashmaps

40% of companies test this subject.

## Qué son

Un **diccionario** (mapa, hash map, tabla hash) es una estructura que asocia **claves** a **valores**. La implementación típica con función hash permite búsqueda, inserción y borrado en tiempo promedio O(1). Las claves suelen ser únicas.
s
## Conceptos clave

- **Clave–valor:** cada clave tiene como máximo un valor asociado.
- **Función hash:** transforma la clave en un índice del array interno; colisiones se resuelven con listas enlazadas o direccionamiento abierto.
- **Orden:** en Java `HashMap` no garantiza orden; `LinkedHashMap` mantiene orden de inserción; `TreeMap` orden por clave (O(log n) por operación).
- **Conteo de frecuencias:** patrón muy común: clave = elemento, valor = cuántas veces aparece.
- **Comprobación de existencia:** saber si un elemento ya se ha visto (clave presente) en O(1).

## Datos clave

| Operación      | HashMap (promedio) | TreeMap  | Notas                  |
| -------------- | ------------------ | -------- | ---------------------- |
| Búsqueda       | O(1)               | O(log n) | Por clave              |
| Inserción      | O(1)               | O(log n) |                        |
| Borrado        | O(1)               | O(log n) |                        |
| Recorrer todos | O(n)               | O(n)     | n = número de entradas |

En el peor caso (muchas colisiones), un hash map puede degradarse a O(n) por operación; en la práctica esto se mitiga con buena función hash y factor de carga.

## Ejemplo: dos sumas

Dado un array y un valor objetivo, encontrar dos índices cuyos elementos sumen el objetivo. Asumir exactamente una solución.

```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> indexByValue = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (indexByValue.containsKey(complement)) {
            return new int[] { indexByValue.get(complement), i };
        }
        indexByValue.put(nums[i], i);
    }
    return new int[] {};
}
```

Complejidad: O(n) tiempo, O(n) espacio.

## Ejemplo: conteo de frecuencias

Contar cuántas veces aparece cada carácter en una cadena.

```java
Map<Character, Integer> countChars(String s) {
    Map<Character, Integer> freq = new HashMap<>();
    for (char c : s.toCharArray()) {
        freq.merge(c, 1, Integer::sum);
    }
    return freq;
}
```

## Patrones frecuentes

- Usar el mapa para “recordar” índices o conteos en un solo recorrido.
- Subarray con suma/contenido dado: prefijos + mapa de “suma de prefijo” → índice.
- Agrupar por alguna clave (anagramas: clave = cadena ordenada o histograma).
- Comprobar duplicados o elementos en común entre dos colecciones.
