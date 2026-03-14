# Flutter: navegación y preservación de datos

La navegación en Flutter permite pasar de una pantalla a otra (push/pop) o usar un router declarativo. La preservación de datos evita perder el estado de pantallas al navegar o al rotar; se combina con el estado (Provider, etc.) y opciones como PageStorageKey o restauración de rutas.

## Navegación (Navigator 1.x)

- **Push:** `Navigator.of(context).push(MaterialPageRoute(builder: (ctx) => PantallaB()));`
- **Pop:** `Navigator.of(context).pop();` (opcionalmente con valor de retorno).
- **Push y reemplazar:** `pushReplacement` para no volver atrás a la pantalla actual.

Las rutas se apilan; `pop` quita la actual y vuelve a la anterior.

## Navegación declarativa (Navigator 2 / GoRouter)

Con **go_router** (o similar) las rutas se definen por URL y estado del router:

```dart
final router = GoRouter(
  routes: [
    GoRoute(path: '/', builder: (_, __) => Home()),
    GoRoute(path: '/detalle/:id', builder: (_, state) => Detalle(id: state.pathParameters['id'])),
  ],
);
// Navegar: context.go('/detalle/1'); o context.push('/detalle/1');
```

Ventajas: deep links, rutas con parámetros, y un solo lugar donde se define el árbol de rutas.

## Preservación de estado

- **PageStorageKey:** Asignar la misma clave a widgets que deben conservar scroll o estado interno al cambiar de pestaña o al volver a la ruta (p. ej. en un `PageView` o listas por pestaña).
- **Estado global:** Mantener el estado de pantallas en Provider/Riverpod/Bloc para que al volver la pantalla recupere los datos.
- **Restauración:** `RestorableRouteFuture` y APIs de restauración de Flutter para que el sistema restablezca la pila de rutas tras cierre del proceso (p. ej. en Android).

En resumen: Navigator 1 para flujos simples; GoRouter (u otro router declarativo) para rutas complejas y deep links; PageStorageKey y estado global para no perder datos al navegar.

---

[← Volver al README principal](../../README.md)
