# Clean Code

Clean Code es la práctica de escribir código legible, mantenible y expresivo. El objetivo es que cualquier desarrollador entienda la intención con poco esfuerzo, se reduzcan errores y los cambios futuros sean más seguros y baratos.

## Principios resumidos

| Área              | Idea clave                                                              |
| ----------------- | ----------------------------------------------------------------------- |
| Nombres           | Descriptivos, pronunciables; evitar abreviaturas oscuras                |
| Funciones         | Cortas, una sola responsabilidad, pocos argumentos                      |
| Comentarios       | Explicar el "por qué", no el "qué"; evitar comentarios obsoletos        |
| Formato           | Indentación y estilo consistentes; bloques pequeños                     |
| Manejo de errores | Usar excepciones; no devolver códigos de error mezclados con resultados |
| Tests             | Código de prueba legible y mantenible; refactorizar sin miedo           |

---

## Nombres con significado

Los nombres de variables, métodos y clases deben revelar su propósito. Evitar `data`, `info`, `tmp` o letras sueltas (salvo índices muy locales). El código se lee muchas más veces de las que se escribe.

**Ejemplo (vida real):** En una receta, "mezclar hasta que espese" es más claro que "mezclar hasta estado 3".

**Ejemplo (código):**

```ts
// Poco claro
const d = new Date();
const x = usuarios.filter((u) => u.a > 18);

// Más claro
const fechaActual = new Date();
const usuariosMayoresDeEdad = usuarios.filter((u) => u.edad > 18);
```

---

## Funciones pequeñas y enfocadas

Cada función debe hacer una sola cosa y hacerla bien. Preferir nombres que describan esa única acción. Pocos parámetros (idealmente ≤ 3); si hay muchos, considerar un objeto de parámetros. Menos anidación (if/for dentro de if) mejora la legibilidad.

**Ejemplo (código):**

```ts
// Mejor: una responsabilidad por función
function tieneDescuento(cliente: Cliente): boolean {
  return cliente.esVip || cliente.antiguedadAnios >= 5;
}

function calcularPrecioFinal(precio: number, cliente: Cliente): number {
  const factor = tieneDescuento(cliente) ? 0.9 : 1;
  return precio * factor;
}
```

---

## Comentarios útiles

Los comentarios no sustituyen código claro. Preferir código autodocumentado. Usar comentarios para explicar decisiones de negocio, advertencias, requisitos no obvios o "por qué" se hizo algo de una forma concreta. Eliminar comentarios que repitan lo que ya dice el código o que estén desactualizados.

---

## Tests y refactor

El código limpio es fácil de probar. Tests legibles (estructura tipo AAA: Arrange, Act, Assert) actúan como documentación y permiten refactorizar con confianza. Mantener los tests tan limpios como el código de producción.

---

[← Volver al README principal](../../README.md)
