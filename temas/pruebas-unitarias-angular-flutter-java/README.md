# Pruebas unitarias en Angular, Flutter y Java

Las pruebas unitarias verifican unidades de código (clases, funciones, componentes) de forma aislada, normalmente con dependencias sustituidas por mocks. Cada stack ofrece herramientas y convenciones propias; el patrón AAA (Arrange, Act, Assert) aplica en todos.

## Resumen por stack

| Stack       | Herramientas típicas                              | Ubicación / comando                  |
| ----------- | ------------------------------------------------- | ------------------------------------ |
| **Angular** | Jasmine (o Jest), Karma; TestBed para componentes | `*.spec.ts`; `ng test`               |
| **Flutter** | `test`, `flutter_test`; mocks con `mockito`       | `test/`; `flutter test`              |
| **Java**    | JUnit 5, Mockito                                  | `src/test/java`; `mvn test` / Gradle |

## Angular

- **Servicios:** Se inyectan en el test; dependencias con `jasmine.createSpyObj()` o clases mock.
- **Componentes:** `TestBed.configureTestingModule` con declaraciones e imports; se puede compilar solo el componente (standalone) o el módulo.
- **Async:** `fakeAsync`/`tick` o `async`/`await` para operaciones asíncronas; HTTP con `HttpClientTestingModule` y `HttpTestingController`.

```ts
it("should add numbers", () => {
  const service = TestBed.inject(CalculatorService);
  expect(service.add(2, 3)).toBe(5);
});
```

## Flutter

- **Unidades:** `test('description', () { ... })`; `expect(result, equals(expected))`.
- **Widgets:** `testWidgets` con `pumpWidget`, `find`, `tap`, `pump` para simular interacción.
- **Mocks:** `@GenerateMocks` con `build_runner` o mocks manuales que implementen la interfaz.

```dart
test('add returns sum', () {
  expect(Calculator().add(2, 3), 5);
});
```

## Java

- **JUnit 5:** `@Test`, `@BeforeEach`, `assertEquals`, `assertThrows`.
- **Mockito:** `@ExtendWith(MockitoExtension.class)`, `@Mock`, `@InjectMocks`; `when().thenReturn()`, `verify()`.

```java
@Test
void shouldAddNumbers() {
    var calc = new Calculator();
    assertEquals(5, calc.add(2, 3));
}
```

En los tres entornos conviene seguir AAA, mantener tests rápidos y aislados, y evitar lógica compleja en los tests. Ver también [Pruebas](../pruebas/README.md) y [TDD/BDD](../tdd-bdd/README.md) en este repositorio.

---

[← Volver al README principal](../../README.md)
