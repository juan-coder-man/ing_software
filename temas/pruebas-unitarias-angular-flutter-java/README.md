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

## Pasos para implementar pruebas

### Angular

1. **Configurar el proyecto:** Las pruebas vienen con el CLI (`ng test`). Si usas Jest, instalar `jest`, `jest-preset-angular` y configurar `jest.config.js`.
2. **Crear el archivo de test:** Junto al archivo bajo prueba, crear `nombre.spec.ts` (p. ej. `calculator.service.spec.ts`).
3. **Configurar el módulo de pruebas:** En tests de componentes, usar `TestBed.configureTestingModule({ declarations, imports, providers })`; para servicios, inyectar con `TestBed.inject(Service)`.
4. **Sustituir dependencias:** Usar `jasmine.createSpyObj('Service', ['metodo'])` o clases mock; para HTTP, importar `HttpClientTestingModule` y usar `HttpTestingController` para simular respuestas.
5. **Escribir el test en AAA:** Arrange (datos y mocks), Act (llamar al método o disparar la acción), Assert (`expect(...).toBe(...)`).
6. **Ejecutar:** `ng test` (o `npm run test` si está configurado Jest).

### Flutter

1. **Añadir dependencias:** En `pubspec.yaml`, incluir `flutter_test` (ya viene con el SDK) y, si usas mocks, `mockito` y `build_runner` en `dev_dependencies`.
2. **Crear la carpeta y archivo:** Crear `test/` en la raíz y archivos como `calculator_test.dart` (o `widget_test.dart` para widgets).
3. **Importar:** `import 'package:flutter_test/flutter_test.dart';` y el archivo/código bajo prueba.
4. **Escribir tests de unidad:** `test('descripción', () { expect(Calculator().add(2, 3), 5); });`.
5. **Tests de widgets (opcional):** Usar `testWidgets`, `pumpWidget(MiWidget())`, `find.byType`, `find.text`, `tester.tap`, `tester.pump` y `expect(find.text('...'), findsOneWidget)`.
6. **Mocks (opcional):** Anotar con `@GenerateMocks([MiClase])` y ejecutar `dart run build_runner build`; en el test usar `MockMiClase()` y configurar con `when(...).thenReturn(...)`.
7. **Ejecutar:** `flutter test` o `flutter test test/nombre_test.dart`.

### Java

1. **Añadir dependencias:** En Maven (`pom.xml`) o Gradle, incluir JUnit 5 (`junit-jupiter`) y Mockito (`mockito-junit-jupiter`) en ámbito `test`.
2. **Estructura de carpetas:** Crear `src/test/java` con el mismo paquete que el código bajo prueba (p. ej. `com.app.service`).
3. **Crear la clase de test:** Nombrar `NombreClaseTest` o `NombreClaseIT` para integración; anotar con `@ExtendWith(MockitoExtension.class)` si usas Mockito.
4. **Preparar mocks e instancia:** `@Mock` para dependencias, `@InjectMocks` para la clase bajo prueba; opcionalmente `@BeforeEach` para datos comunes.
5. **Escribir el test:** Anotar con `@Test`, seguir AAA: configurar mocks con `when(mock.metodo()).thenReturn(valor)`, ejecutar el método, afirmar con `assertEquals`, `assertThrows`, etc.; usar `verify(mock.metodo()).called(n)` si aplica.
6. **Ejecutar:** `mvn test` o `./gradlew test`; para un solo test, `mvn test -Dtest=NombreClaseTest#nombreMetodo`.

---

[← Volver al README principal](../../README.md)
