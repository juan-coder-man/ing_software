# Paradigmas de Programación

Un paradigma de programación es un modelo o estilo que define cómo se estructura el código y cómo se resuelven los problemas. Los lenguajes suelen apoyar varios paradigmas; elegir el adecuado (o combinarlos) mejora la claridad y el mantenimiento.

## Tabla resumen

| Paradigma                 | Unidad central             | Enfoque                                                    | Ejemplo típico                                    |
| ------------------------- | -------------------------- | ---------------------------------------------------------- | ------------------------------------------------- |
| Imperativo                | Secuencia de instrucciones | "Cómo" paso a paso                                         | C, Pascal, scripts en Bash                        |
| Orientado a objetos (OOP) | Objetos y clases           | Encapsulación, herencia, polimorfismo                      | Java, C#, TypeScript                              |
| Funcional                 | Funciones y composición    | Inmutabilidad, funciones puras, evitar efectos secundarios | Haskell, Clojure; en Java/JS: lambdas, map/filter |
| Declarativo               | Qué se quiere, no cómo     | Descripción del resultado                                  | SQL, HTML, lenguajes de configuración             |

---

## Imperativo

El programa describe una secuencia de comandos que modifican estado (variables). El flujo se controla con bucles, condicionales y asignaciones. Es el estilo más cercano a la máquina.

**Ejemplo (código):** Sumar los pares de una lista:

```ts
const numeros = [1, 2, 3, 4, 5];
let suma = 0;
for (let i = 0; i < numeros.length; i++) {
  if (numeros[i] % 2 === 0) suma += numeros[i];
}
```

---

## Orientado a objetos (OOP)

El diseño se organiza en objetos que encapsulan datos y comportamiento. Se usan clases, herencia, polimorfismo y abstracción para modelar el dominio y reutilizar código. Ver [POO](../poo/README.md) en este repositorio.

**Ejemplo (código):** Mismo resultado con métodos de objeto:

```ts
class Calculadora {
  sumarPares(numeros: number[]): number {
    return numeros.filter((n) => n % 2 === 0).reduce((a, b) => a + b, 0);
  }
}
```

---

## Funcional

Se priorizan las funciones puras (mismo entrada → misma salida, sin efectos secundarios), la inmutabilidad y la composición de funciones. Se evitan bucles mutando variables; se usan map, filter, reduce y recursión.

**Ejemplo (código):** Misma suma de pares de forma funcional:

```ts
const sumarPares = (numeros: number[]) =>
  numeros.filter((n) => n % 2 === 0).reduce((a, b) => a + b, 0);
```

---

## Declarativo

Se describe _qué_ se quiere obtener, no _cómo_ calcularlo paso a paso. La implementación concreta la resuelve el motor (base de datos, navegador, framework). SQL y HTML son ejemplos típicos.

**Ejemplo:** En SQL no se escriben bucles para recorrer filas; se declara el resultado:

```sql
SELECT SUM(valor) FROM numeros WHERE valor % 2 = 0;
```

---

[← Volver al README principal](../../README.md)
