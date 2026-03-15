# Flutter: deep links

Los deep links permiten abrir la app (o una pantalla concreta) desde un enlace externo (email, web, notificación). En Flutter se configuran en **iOS** (Universal Links / URL Scheme) y **Android** (Intent filters / App Links) y luego se manejan en la app, normalmente con un router como GoRouter.

## Conceptos

| Tipo                                                          | Descripción                                                                                                                                 |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **URL Scheme** (iOS) / **Intent con custom scheme** (Android) | Enlaces como `miapp://ruta/detalle/1`. Requieren que la app esté instalada.                                                                 |
| **Universal Links** (iOS) / **App Links** (Android)           | Enlaces HTTP(S) que abren la app si está instalada (p. ej. `https://midominio.com/detalle/1`). Mejor experiencia y verificación de dominio. |

## Cómo sabe la app a dónde ir

Flujo desde que el usuario abre el enlace hasta que se muestra la pantalla:

1. **Usuario toca el enlace** (navegador, email, notificación).
2. **El SO resuelve el enlace:** Si es App Link / Universal Link y la app está instalada, abre la app y le entrega la URI (Intent en Android, NSUserActivity/URL en iOS).
3. **En Flutter, un plugin recibe la URI:** `app_links` o `uni_links` obtienen la URI: con **getInitialLink()** si la app se abrió desde el enlace, o con el stream de enlaces si la app estaba en segundo plano.
4. **La app parsea la URI:** Se usan `uri.path` y `uri.queryParameters` (p. ej. `/detalle/123` o `?id=123`).
5. **Mapeo path → ruta del router:** Se traduce el path a la ruta interna del router (p. ej. GoRouter con `path: '/detalle/:id'`) y se llama a `context.go()` o `router.go()` con esa ruta.
6. **El router muestra la pantalla:** Construye la pila de rutas y el widget correspondiente (p. ej. pantalla de detalle que lee el `id` del route y carga el recurso).

Ejemplo concreto: enlace `https://midominio.com/app/detalle/5` → la app recibe `path = /app/detalle/5` (o el path que exponga el plugin según el host/pathPrefix) → GoRouter hace match con `/detalle/:id` → navega a la pantalla de detalle con `id = 5`, que usa el parámetro para cargar el recurso.

## Android (AndroidManifest.xml)

Para **App Links** (HTTPS):

```xml
<intent-filter android:autoVerify="true">
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data android:scheme="https" android:host="midominio.com" android:pathPrefix="/app" />
</intent-filter>
```

Para **custom scheme**: mismo `intent-filter` pero con `<data android:scheme="miapp" android:host="..." />`. En el proyecto Flutter suele usarse el plugin `android_intent_plus` o la configuración nativa que expone el template.

## iOS (Info.plist / Xcode)

Para **URL Scheme**: en Xcode → Target → Info → URL Types: Scheme `miapp`, Identifier único.

Para **Universal Links**: asociar el dominio en Apple Developer (Associated Domains) y añadir el entitlement; el servidor debe servir `apple-app-site-association` en `/.well-known/` o `/.well-known/apple-app-site-association`.

## Flutter: recibir el enlace

Con **go_router** se puede definir la ruta que coincide con el path del deep link; al abrir la app con el enlace, el sistema pasa la URL y el router navega. También se usa **uni_links** o **app_links** para obtener la URL inicial y las que llegan con la app en segundo plano. Ejemplo con `app_links`:

```dart
final appLinks = AppLinks();
Uri? initial = await appLinks.getInitialLink();
appLinks.uriLinkStream.listen((Uri uri) { /* navegar según uri.path */ });
```

Configurar las rutas del router para que coincidan con los paths (p. ej. `/detalle/:id`) permite que un solo enlace abra la pantalla correcta.

---

[← Volver al README principal](../../README.md)
