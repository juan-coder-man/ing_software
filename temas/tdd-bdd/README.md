# TDD y BDD

TDD (Desarrollo guiado por pruebas) y BDD (Desarrollo guiado por comportamiento) son enfoques en los que las pruebas influyen o definen el diseño. TDD se centra en pruebas unitarias escritas por el desarrollador; BDD en escenarios en lenguaje natural compartidos por negocio y desarrollo.

## TDD (Test-Driven Development)

El ciclo es **Red → Green → Refactor**:

1. **Red:** Escribir una prueba que falle (el comportamiento aún no existe).
2. **Green:** Escribir el mínimo código para que la prueba pase.
3. **Refactor:** Mejorar el código manteniendo las pruebas en verde.

Beneficios: diseño más desacoplado, código cubierto por pruebas desde el inicio, documentación ejecutable. Se aplica sobre todo a unidades (clases, funciones).

**Ejemplo de ciclo:** Quieres una función `suma(a, b)`. Primero escribes `expect(suma(2, 3)).toBe(5);` (falla). Implementas `return a + b;` (pasa). Refactorizas nombres o estructura si hace falta.

## BDD (Behavior-Driven Development)

BDD extiende la idea de TDD con escenarios escritos en lenguaje natural (habitualmente **Gherkin**), entendibles por negocio y automatizables:

```gherkin
Feature: Login
  Scenario: Usuario con credenciales válidas
    Given el usuario está en la página de login
    When introduce email "user@test.com" y contraseña correcta
    Then ve el dashboard
```

Las herramientas (Cucumber, SpecFlow, etc.) enlazan estos pasos con código que ejecuta la prueba. Los escenarios sirven como especificación y como pruebas de aceptación.

## Relación TDD y BDD

| Enfoque | Nivel típico                   | Lenguaje                   | Quién escribe        |
| ------- | ------------------------------ | -------------------------- | -------------------- |
| **TDD** | Unitario / integración técnica | Código (JUnit, Jest, etc.) | Desarrolladores      |
| **BDD** | Aceptación / flujos de usuario | Gherkin (Given/When/Then)  | Negocio + desarrollo |

BDD no sustituye TDD: se pueden usar ambos (TDD para unidades, BDD para criterios de aceptación y flujos clave).

---

[← Volver al README principal](../../README.md)
