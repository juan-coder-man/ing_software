# Flutter: tipos de compilación y despliegue iOS/Android

Flutter permite compilar en **debug**, **profile** y **release**. Para publicar en App Store o Google Play se usa **release**; el proceso implica firma, configuración de versión y subida a las tiendas.

## Tipos de compilación

| Modo        | Uso               | Características                                                                     |
| ----------- | ----------------- | ----------------------------------------------------------------------------------- |
| **Debug**   | Desarrollo diario | Hot reload, asserts, símbolos; más lento y pesado.                                  |
| **Profile** | Medir rendimiento | Sin hot reload, con profiling; similar a release pero con herramientas de análisis. |
| **Release** | Producción        | Optimizado, sin asserts ni símbolos de depuración; tamaño y velocidad óptimos.      |

Comandos: `flutter run` (debug), `flutter run --profile`, `flutter run --release`. Para generar artefactos: `flutter build apk`, `flutter build appbundle`, `flutter build ios`.

## Build para Android

- **APK:** `flutter build apk` (para pruebas o distribución directa). Variantes: `--split-per-abi` para reducir tamaño por dispositivo.
- **App Bundle:** `flutter build appbundle` (recomendado para Play Store); Google genera APKs optimizados por dispositivo.
- **Firma:** Configurar `key.properties` y `build.gradle` con keystore y alias; en release el build usa esa firma.

## Build para iOS

- **Requisitos:** Mac, Xcode, cuenta Apple Developer, certificados y provisioning profile.
- **Comando:** `flutter build ios`; luego abrir `ios/Runner.xcworkspace` en Xcode para archivar y subir a App Store Connect (o distribuir por TestFlight).
- **Configuración:** Versión y build number en `pubspec.yaml` o en Xcode; permisos y capacidades en el target.

## Despliegue en tiendas

- **Google Play:** Subir el AAB desde Play Console; completar ficha, políticas y revisión.
- **App Store:** Subir el build desde Xcode (Archive → Distribute) a App Store Connect; completar ficha, revisión y envío.

Buenas prácticas: versionado semántico o incremental (version + build number), pruebas en dispositivos reales, cumplir guías de cada tienda y revisar permisos y privacidad antes del envío.

---

[← Volver al README principal](../../README.md)
