# Flutter: ciclo de vida del widget y de la app

En Flutter es útil distinguir el ciclo de vida de un **widget** (sobre todo StatefulWidget) del ciclo de vida de la **aplicación** (cuando pasa a segundo plano o se cierra). Conocer ambos ayuda a inicializar recursos, suscribirse a streams o limpiar en el momento correcto.

## Ciclo de vida de un StatefulWidget

| Método / fase               | Cuándo se llama                                                                                                                                      |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **createState()**           | El framework crea el objeto State.                                                                                                                   |
| **initState()**             | Una vez; ideal para suscripciones, controladores, datos iniciales. No usar `context` que dependa de un padre ya construido (p. ej. navegación) aquí. |
| **didChangeDependencies()** | Tras initState y cuando cambian dependencias (InheritedWidget).                                                                                      |
| **build()**                 | Cada vez que el widget debe reconstruirse (setState, cambio de padre, etc.).                                                                         |
| **didUpdateWidget()**       | El widget padre reconstruyó con nueva configuración (nuevo Widget, mismo State).                                                                     |
| **deactivate()**            | El State se quita del árbol temporalmente (p. ej. navegación).                                                                                       |
| **dispose()**               | Una vez, al eliminar el State de forma definitiva; cancelar controladores, suscripciones, listeners.                                                 |

### Qué dispara un rebuild

El **build()** se ejecuta cuando:

1. **setState()** — Se llama desde el mismo `State`; el framework marca el widget como “dirty” y programa un rebuild en el siguiente frame.
2. **Cambio de dependencias (InheritedWidget)** — Si un `InheritedWidget` del que depende este widget actualiza (p. ej. `Provider` notifica), primero se llama `didChangeDependencies()` y luego `build()`.
3. **El padre reconstruye con un Widget nuevo** — Mismo tipo de widget pero instancia distinta; se llama `didUpdateWidget()` y después `build()`.
4. **Inserción en el árbol** — La primera vez que el widget se monta, tras `initState()` y `didChangeDependencies()`, se llama `build()`.

Regla práctica: **initState** para crear; **dispose** para limpiar. No llamar `setState` en dispose ni después de dispose.

## Ciclo de vida de la aplicación

Con **WidgetsBindingObserver** se reciben notificaciones cuando la app pasa a segundo plano o vuelve:

```dart
class MiApp extends StatefulWidget {
  @override
  _MiAppState createState() => _MiAppState();
}
class _MiAppState extends State<MiApp> with WidgetsBindingObserver {
  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
  }
  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    if (state == AppLifecycleState.paused) { /* segundo plano */ }
    if (state == AppLifecycleState.resumed) { /* vuelve al frente */ }
  }
}
```

Estados típicos: `resumed`, `inactive`, `paused`, `detached`, `hidden`. Útil para pausar animaciones, guardar estado o liberar recursos al ir a segundo plano.

---

[← Volver al README principal](../../README.md)
