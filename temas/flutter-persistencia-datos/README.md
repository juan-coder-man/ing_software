# Flutter: persistencia de datos

En Flutter hay varias formas de persistir datos en el dispositivo: desde clave-valor y archivos hasta bases de datos locales. La elección depende del tipo de dato, del volumen y de si la información es sensible (en ese caso ver [Flutter: almacenamiento seguro](../flutter-almacenamiento-seguro/README.md)).

## SharedPreferences

Almacenamiento clave-valor persistente entre sesiones. API básica: **setString**, **getString**, **setInt**, **getInt**, **setBool**, **getBool**, **setStringList**, **getStringList**, y **remove** / **clear**. Los datos se escriben de forma asíncrona.

**Uso típico:** Preferencias de usuario (tema, idioma, flags de onboarding). **Limitaciones:** Solo tipos simples; no está pensado para datos grandes ni estructurados; no es seguro para secretos.

```dart
import 'package:shared_preferences/shared_preferences.dart';

final prefs = await SharedPreferences.getInstance();
await prefs.setString('tema', 'oscuro');
String? tema = prefs.getString('tema');
```

## Archivos (path_provider + File)

**path_provider** da rutas a directorios del sistema: **getApplicationDocumentsDirectory** (persistente, respaldado), **getTemporaryDirectory** (puede borrarse). Con **dart:io** **File** se lee y escribe en esas rutas.

**Uso típico:** Archivos binarios, JSON guardado a mano, exportaciones, caché de ficheros. No usar para secretos; para eso usar almacenamiento seguro.

```dart
import 'package:path_provider/path_provider.dart';
import 'dart:io';

final dir = await getApplicationDocumentsDirectory();
final file = File('${dir.path}/datos.json');
await file.writeAsString(jsonEncode(miMap));
```

## Bases de datos locales

### sqflite

SQLite en Flutter. Se abre una base de datos, se crean tablas con SQL y se hacen **INSERT**, **UPDATE**, **DELETE**, **SELECT**. La API es asíncrona (**openDatabase**, **rawQuery**, **insert**, etc.).

**Cuándo usarla:** Datos relacionales, muchas filas, consultas SQL complejas o reportes.

### Hive / Isar / ObjectBox

Bases de datos NoSQL o orientadas a objetos en el dispositivo. **Hive:** clave-valor o cajas con tipos; muy ligera. **Isar** y **ObjectBox:** almacenan objetos Dart con índices y consultas; buen rendimiento y soporte para relaciones. Útiles cuando se trabaja con objetos Dart y se quiere evitar escribir SQL a mano.

## Qué usar según el caso

| Necesidad                             | Opción recomendada                                                  |
| ------------------------------------- | ------------------------------------------------------------------- |
| Preferencias, flags, configuración    | SharedPreferences                                                   |
| Tokens, contraseñas, datos sensibles  | [Almacenamiento seguro](../flutter-almacenamiento-seguro/README.md) |
| Datos estructurados pequeños          | Hive o SharedPreferences (JSON como string)                         |
| Datos relacionales o muchos registros | sqflite o Isar                                                      |
| Archivos, blobs, exportaciones        | File + path_provider                                                |

Para tokens y datos sensibles, usar siempre almacenamiento seguro (p. ej. **flutter_secure_storage**); no guardar secretos en SharedPreferences ni en archivos en claro.

---

[← Volver al README principal](../../README.md)
