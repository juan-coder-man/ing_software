# Pruebas

Las pruebas de software verifican que el código se comporta como se espera y reducen el riesgo al cambiar o extender funcionalidad. Se suelen clasificar por nivel (unitarias, integración, funcionales, end-to-end) y por técnica (caja negra, mocks, AAA).
Ø

## Tipos de pruebas

| Tipo                 | Alcance                                                        | Objetivo                                                                        | Herramientas típicas (Java)          |
| -------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------ |
| **Unitarias**        | Una clase o función aislada                                    | Verificar lógica con dependencias sustituidas (mocks)                           | JUnit 5, Mockito                     |
| **Integración**      | Varios componentes juntos (BD, API)                            | Verificar que las piezas colaboran bien                                         | JUnit 5, Testcontainers, Spring Test |
| **Funcionales**      | Casos de uso o funcionalidades desde la API o capa de servicio | Verificar requisitos funcionales de negocio sin mirar la implementación interna | JUnit 5, Spring Test, RestAssured    |
| **End-to-end (E2E)** | Sistema completo desde la UI o API externa                     | Verificar flujos de usuario o contratos extremo a extremo                       | Selenium, RestAssured, Playwright    |

---

## Estructura AAA

Las pruebas suelen seguir el patrón **Arrange–Act–Assert**:

1. **Arrange:** preparar datos y dependencias (objetos bajo prueba, mocks).
2. **Act:** ejecutar la acción que se quiere probar (una llamada a método o API).
3. **Assert:** comprobar que el resultado o los efectos son los esperados.

**Ejemplo (código):** Prueba unitaria en Java con JUnit 5 y Mockito:

```java
@Test
void deberiaCalcularPrecioConDescuento() {
    // Arrange
    CalculadoraPrecio calc = new CalculadoraPrecio();
    double precioBase = 100;
    double descuento = 10;

    // Act
    double resultado = calc.precioConDescuento(precioBase, descuento);

    // Assert
    assertEquals(90, resultado);
}
```

---

## Mocks y dobles

Para aislar la unidad bajo prueba se sustituyen dependencias por **mocks** (objetos que simulan comportamiento y permiten verificar interacciones). Otras variantes son **stubs** (devuelven valores fijos) y **spies** (objeto real con capacidad de verificar llamadas).

**Ejemplo (código):** Dependencia mockeada:

```java
@Test
void deberiaEnviarEmailAlRegistrar() {
    EmailService emailMock = mock(EmailService.class);
    ServicioRegistro servicio = new ServicioRegistro(emailMock);

    servicio.registrar("user@ejemplo.com", "pass");

    verify(emailMock).enviarBienvenida("user@ejemplo.com");
}
```

---

## Buenas prácticas

- **Nombres descriptivos:** el nombre del test debe indicar el escenario y el resultado esperado (p. ej. `deberiaDevolverNullCuandoUsuarioNoExiste`).
- **Un concepto por test:** cada test verifica un comportamiento; evitar tests que comprueben muchas cosas a la vez.
- **Independencia:** los tests no deben depender del orden de ejecución ni compartir estado mutable.
- **Rapidez:** las unitarias deben ser rápidas; las de integración y E2E pueden ser más lentas pero acotadas.

---

[← Volver al README principal](../../README.md)
