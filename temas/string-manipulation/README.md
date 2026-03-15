# String Manipulation

## Qué son

Las **cadenas de caracteres** (strings) son secuencias inmutables en muchos lenguajes (Java, Python, JavaScript con primitivo). La manipulación incluye: comparación, búsqueda de subcadenas, concatenación, reversión, parsing y uso de estructuras auxiliares (mapas, conjuntos) para conteos o agrupaciones.

## Conceptos clave

- **Inmutabilidad:** en Java y otros, cada “cambio” crea una nueva cadena; concatenar en bucle es O(n²) si se hace con `+` en bucle; usar `StringBuilder` para construir en O(n).
- **Comparación:** `equals` por contenido; `compareTo` por orden lexicográfico; no comparar con `==` si se trata de contenido (solo identidad de referencia en Java).
- **Subcadena:** `substring(inicio, fin)`; índices suelen ser inicio inclusive, fin exclusive.
- **Caracteres:** acceso por índice O(1); recorrer con `charAt(i)` o `toCharArray()`.
- **Anagramas:** mismas letras en otro orden; claves típicas: cadena ordenada o array de frecuencias por letra.
- **Palíndromos:** se lee igual de izquierda a derecha que de derecha a izquierda; dos punteros desde los extremos.

## Datos clave

| Operación            | Complejidad típica | Notas                              |
| -------------------- | ------------------ | ---------------------------------- |
| Acceso por índice    | O(1)               |                                    |
| Longitud             | O(1)               | Almacenada                         |
| Comparación (igual)  | O(n)               | n = longitud                       |
| Substring            | O(k) o O(1)        | k = longitud; depende del lenguaje |
| Concatenar (builder) | O(n + m)           | n, m longitudes                    |
| Búsqueda subcadena   | O(n·m) naive       | KMP y similares O(n + m)           |

## Ejemplo: comprobar palíndromo

```java
boolean isPalindrome(String s) {
    int i = 0, j = s.length() - 1;
    while (i < j) {
        if (s.charAt(i) != s.charAt(j)) return false;
        i++;
        j--;
    }
    return true;
}
```

Ignorar no-alfanuméricos y mayúsculas:

```java
boolean isPalindromeAlnum(String s) {
    int i = 0, j = s.length() - 1;
    while (i < j) {
        while (i < j && !Character.isLetterOrDigit(s.charAt(i))) i++;
        while (i < j && !Character.isLetterOrDigit(s.charAt(j))) j--;
        if (Character.toLowerCase(s.charAt(i)) != Character.toLowerCase(s.charAt(j)))
            return false;
        i++;
        j--;
    }
    return true;
}
```

## Ejemplo: anagramas

Comprobar si dos cadenas son anagramas (mismas letras, mismo número de veces).

```java
boolean areAnagrams(String a, String b) {
    if (a.length() != b.length()) return false;
    int[] count = new int[26];
    for (int i = 0; i < a.length(); i++) {
        count[a.charAt(i) - 'a']++;
        count[b.charAt(i) - 'a']--;
    }
    for (int c : count) if (c != 0) return false;
    return true;
}
```

## Patrones frecuentes

- Dos punteros (inicio/fin) para palíndromos o comparaciones.
- Mapa o array de conteo para frecuencias de caracteres.
- `StringBuilder` para construir resultado sin copias cuadráticas.
- Parsing: recorrer carácter a carácter o por tokens (split, regex) según el formato.
