# Flutter: gestores de estado

En Flutter, el estado (datos que pueden cambiar y deben reflejarse en la UI) se gestiona con distintas aproximaciones: desde el `setState` local hasta soluciones globales o reactivas. La elección depende del tamaño de la app y de si el estado es local al widget o compartido.

## Opciones principales

| Enfoque          | Descripción                                                                           | Cuándo usarlo                                                                       |
| ---------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **setState**     | Actualizar estado local en un `StatefulWidget`; `setState(() { ... })` marca rebuild. | Estado limitado a un widget o un árbol pequeño.                                     |
| **Provider**     | Inyección de dependencias y estado; el widget escucha cambios y se reconstruye.       | Estado compartido sin demasiada complejidad; recomendado por el equipo Flutter.     |
| **Riverpod**     | Evolución de Provider; compilación más segura, sin contexto, fácil de testear.        | Apps que crecen; preferible para nuevo código que requiera Provider-like.           |
| **Bloc / Cubit** | Patrón BLoC: eventos → bloc → estados; Cubit sin eventos, solo métodos.               | Lógica de negocio clara, flujos complejos, separación UI/lógica.                    |
| **GetX**         | Estado reactivo, rutas, inyección y más en un mismo paquete.                          | Prototipos rápidos o equipos que ya lo usan; menos “oficial” que Provider/Riverpod. |

## Ejemplo: setState

```dart
class Counter extends StatefulWidget {
  @override
  _CounterState createState() => _CounterState();
}
class _CounterState extends State<Counter> {
  int _count = 0;
  void _increment() => setState(() => _count++);
  @override
  Widget build(BuildContext context) => Text('$_count');
}
```

## Ejemplo: Provider

```dart
// Proveedor
ChangeNotifierProvider(create: (_) => MiModelo()..cargar());
// Consumidor
context.watch<MiModelo>();  // rebuild cuando notifica
context.read<MiModelo>().accion();  // sin escuchar
```

Para apps medianas o grandes, Riverpod o Bloc/Cubit suelen escalar mejor que solo setState o Provider en árboles muy grandes.

---

[← Volver al README principal](../../README.md)
