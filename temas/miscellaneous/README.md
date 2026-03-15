# Miscellaneous

## Qué son

En esta categoría entran temas que no encajan en una sola estructura o algoritmo: matemática discreta (teoría de números, combinatoria), bits y manipulación a nivel binario, geometría básica, simulaciones, y problemas que combinan varias ideas (greedy + estructuras, DP + grafos, etc.).

## Conceptos clave

- **Manipulación de bits:** AND, OR, XOR, desplazamientos (<<, >>), máscaras; comprobar si el bit i está encendido `(n >> i) & 1`, poner a 1 `n | (1 << i)`, quitar `n & ~(1 << i)`. XOR de un número consigo mismo da 0; XOR con 0 deja el número.
- **Teoría de números:** máximo común divisor (GCD) con Euclides, primalidad, factores, módulo y aritmética modular.
- **Combinatoria:** permutaciones n!, combinaciones C(n,k), principio de inclusión-exclusión; a veces se pide “cuántas formas” (puede ser DP o fórmula).
- **Simulación:** seguir las reglas del enunciado paso a paso (tableros, colas, procesos); cuidado con índices y casos borde.
- **Precomputación:** tablas (primos, factoriales, potencias) para responder en O(1) por consulta.
- **Problemas “adhoc”:** sin algoritmo estándar; leer bien el enunciado y modelar con las estructuras que encajen.

## Datos clave

| Tema       | Herramientas típicas           | Ejemplo de uso                        |
| ---------- | ------------------------------ | ------------------------------------- |
| Bits       | AND, OR, XOR, shift, popcount  | Paridad, único elemento en duplicados |
| GCD        | Algoritmo de Euclides          | Simplificar fracciones, LCM           |
| Módulo     | (a + b) % m, (a \* b) % m      | Evitar overflow, restas modulares     |
| Primalidad | Trial division hasta √n, sieve | Contar primos, factores               |
| Simulación | Bucles, matrices, estado       | Juegos de tablero, colas              |

## Ejemplo: GCD (Euclides)

```java
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

LCM(a, b) = a _ b / gcd(a, b) (usar con cuidado overflow; mejor LCM = a / gcd(a,b) _ b).

## Ejemplo: único elemento cuando todos están duplicados (XOR)

Si todos los números aparecen dos veces excepto uno, XOR de todos cancela los duplicados y deja el único.

```java
int singleNumber(int[] nums) {
    int x = 0;
    for (int n : nums) x ^= n;
    return x;
}
```

## Ejemplo: comprobar si n es potencia de 2

Una potencia de 2 tiene un solo bit a 1: en binario es 100...0. Entonces n > 0 y (n & (n - 1)) == 0 (porque n-1 invierte ese bit y los de la derecha).

```java
boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

## Ejemplo: contar bits a 1 (popcount)

```java
int popcount(int n) {
    int c = 0;
    while (n != 0) {
        c += n & 1;
        n >>>= 1;
    }
    return c;
}
```

## Patrones frecuentes

- “Aparece todo duplicado menos uno” → XOR.
- “Divisibilidad, factores, primos” → GCD, criba, trial division.
- “Cuántas formas / contar bajo restricciones” → combinatoria o DP.
- “Simular paso a paso” → bucles y estado claro; tests con casos pequeños.
- Problemas raros: releer el enunciado, buscar invariantes o propiedades matemáticas simples.
