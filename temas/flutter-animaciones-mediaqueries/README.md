# Flutter: animaciones y MediaQuery

Flutter permite animaciones implícitas (transiciones declarativas) y explícitas (control por tiempo con AnimationController). MediaQuery da acceso al tamaño de pantalla, padding seguro, orientación y tema para adaptar el layout y los estilos.

## Animaciones implícitas

Widgets que animan un cambio de valor (posición, opacidad, tamaño) sin definir duración manualmente en cada caso:

- **AnimatedContainer**, **AnimatedOpacity**, **AnimatedPositioned**: cambian una propiedad y Flutter interpola.
- **TweenAnimationBuilder**: defines un `Tween` y un `builder`; útil para animaciones simples con un solo valor.

## Animaciones explícitas

Para secuencias o control fino se usa **AnimationController** (con un **Ticker**) y **Tween**:

```dart
class _MiEstado extends State<MiWidget> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animacion;
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(duration: Duration(seconds: 1), vsync: this);
    _animacion = Tween<double>(begin: 0, end: 1).animate(_controller);
  }
  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
  // _controller.forward(); _controller.reverse();
}
```

**vsync** (TickerProvider) sincroniza con los frames; en State se suele usar `SingleTickerProviderStateMixin` o `TickerProviderStateMixin` si hay varios controladores.

## MediaQuery

**MediaQuery.of(context)** expone:

- **size:** ancho y alto de la pantalla (o ventana).
- **padding:** insets seguros (notch, barra de estado).
- **orientation:** portrait / landscape.
- **textScaler**, **brightness**, etc.

```dart
final media = MediaQuery.of(context);
final ancho = media.size.width;
final padding = media.padding;
final esVertical = media.orientation == Orientation.portrait;
```

Se usa para layouts responsive (columnas según ancho, márgenes según padding) y para no dibujar bajo el notch usando `SafeArea` o el padding de MediaQuery.

## Animaciones más avanzadas

- **Hero:** Transición compartida de un widget entre dos pantallas (p. ej. una imagen de una lista que “vuela” a la pantalla de detalle). Se envuelve el widget en **Hero** con un `tag` único; en la pantalla de destino otro **Hero** con el mismo `tag` recibe la transición. Flutter anima automáticamente posición y tamaño entre origen y destino.

- **Animaciones escalonadas (staggered):** Secuenciar varias animaciones (entrada de textos, iconos, etc.). Se puede usar un solo **AnimationController** y varios **Tween** con **Interval**(begin, end) para que cada uno avance en un tramo del ciclo, o varios controladores que se inician con retraso (p. ej. `Future.delayed` o `TickerFuture`).

- **Animaciones con física:** En lugar de curvas puramente temporales, se usa **Simulation** (p. ej. **SpringSimulation**, **GravitySimulation**) y **AnimationController.drive** con un `SimulationTween` para movimientos más naturales (rebotes, caídas).

- **Sliver y listas:** **SliverAnimatedList** permite añadir o quitar ítems de una lista con animación, usando un **AnimatedListSliver** y un **GlobalKey&lt;SliverAnimatedListState&gt;** para llamar a `insertItem` / `removeItem`.

- **Rive / Lottie:** Animaciones vectoriales diseñadas en Rive o After Effects (exportadas a Lottie). Los paquetes **rive** y **lottie** permiten reproducirlas en Flutter. Útiles para animaciones complejas o muy ligadas al diseño, sin escribir todo el código a mano.

- **CustomPainter + AnimationController:** Para animaciones totalmente personalizadas se usa un **CustomPainter** que dibuja en un **Canvas** y se repinta cada frame; el **AnimationController** aporta el valor de animación (0..1 o el que se necesite) y en el paint se usa ese valor para calcular posiciones, colores, etc.

---

[← Volver al README principal](../../README.md)
