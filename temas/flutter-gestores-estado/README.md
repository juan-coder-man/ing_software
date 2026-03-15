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

## En detalle: setState

**Mecanismo:** Al llamar `setState(() { ... })` desde el `State`, el framework marca ese widget como “dirty”; en el siguiente frame se invoca `build()` de ese State y se reconstruye el árbol de descendientes. **Ámbito:** Solo ese State y sus hijos; no sirve para compartir estado con widgets en otra rama del árbol. **Ventajas:** Sin dependencias, fácil de entender. **Límites:** No escalar bien cuando el estado debe llegar a muchos widgets o cuando hay que pasarlo hacia arriba (lifting de estado).

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

## En detalle: Provider

**Idea:** Un “proveedor” (p. ej. **ChangeNotifierProvider**) colocado arriba en el árbol expone un objeto (típicamente un **ChangeNotifier**); los widgets descendientes lo consumen. Al llamar **notifyListeners()** en ese objeto, todos los que usan **context.watch&lt;T&gt;** se reconstruyen; **context.read&lt;T&gt;** solo obtiene la instancia sin escuchar. **Consumer** y **Select** permiten acotar qué parte del estado provoca rebuild. **Cuándo usarlo:** Estado compartido en una zona del árbol, sin necesidad de inyección muy compleja.

```dart
// Proveedor
ChangeNotifierProvider(create: (_) => MiModelo()..cargar());
// Consumidor
context.watch<MiModelo>();   // rebuild cuando notifica
context.read<MiModelo>().accion();   // sin escuchar
```

## En detalle: Riverpod

**Sin BuildContext:** Los “providers” se definen como variables globales (o en un archivo de providers); el widget accede con **ref.watch** / **ref.read** desde un **ConsumerWidget** o usando **Consumer** / **Ref** en un widget que recibe `ref`. Tipos de provider: **Provider**, **StateNotifierProvider**, **FutureProvider**, **StreamProvider**. **Ventajas:** Testeable (se sobreescriben providers), compilación más segura, no depende del árbol de widgets. **Cuándo usarlo:** Apps que crecen o nuevo código que necesite un enfoque tipo Provider pero más escalable.

```dart
final contadorProvider = StateNotifierProvider<ContadorNotifier, int>((ref) => ContadorNotifier());

class ContadorNotifier extends StateNotifier<int> {
  ContadorNotifier() : super(0);
  void incrementar() => state++;
}

// En el widget
ref.watch(contadorProvider);   // rebuild cuando cambia
ref.read(contadorProvider.notifier).incrementar();
```

## En detalle: Bloc / Cubit

**Bloc:** Los eventos se envían con **bloc.add(evento)**; el Bloc los procesa (a menudo con **mapEventToState** o la API actual) y emite nuevos estados. La UI escucha con **BlocBuilder** o **BlocListener**. **Cubit:** Sin eventos; el widget llama métodos del Cubit que emiten estados con **emit(nuevoEstado)**. **Cuándo usarlo:** Lógica de negocio clara, muchos estados o transiciones, y cuando se quiere separar bien UI de reglas de negocio.

```dart
// Cubit mínimo
class ContadorCubit extends Cubit<int> {
  ContadorCubit() : super(0);
  void incrementar() => emit(state + 1);
}
// En el widget
BlocProvider(create: (_) => ContadorCubit(), child: ...);
BlocBuilder<ContadorCubit, int>(builder: (context, count) => Text('$count'));
```

## En detalle: GetX

**Estado reactivo:** Variables con **.obs** y widgets **Obx** que se reconstruyen cuando cambia alguna variable observada. **GetxController** tiene ciclo de vida (onInit, onClose) y se usa con **Get.put** / **Get.find**. Incluye también rutas (**GetPage**, **Get.to**) e inyección. **Ventajas:** Muy rápido de escribir y poco boilerplate. **Consideraciones:** Menos alineado con la documentación oficial de Flutter; en proyectos grandes puede costar más mantener la estructura.

```dart
final count = 0.obs;
void increment() => count++;

// En el widget
Obx(() => Text('${count.value}'));
```

Para apps medianas o grandes, Riverpod o Bloc/Cubit suelen escalar mejor que solo setState o Provider en árboles muy grandes.

---

[← Volver al README principal](../../README.md)
