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

---

[← Volver al README principal](../../README.md)
