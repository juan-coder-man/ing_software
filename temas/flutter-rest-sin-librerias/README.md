# Flutter: consumir REST sin librerías (o con el mínimo)

En Flutter se puede consumir APIs REST usando el paquete **`http`** (oficial y ligero) o el **`HttpClient`** de `dart:io`. No hace falta una librería “REST” pesada para GET/POST y parsear JSON; con `http` + `dart:convert` suele ser suficiente.

## Con el paquete `http`

El paquete `http` es el estándar recomendado; no es una librería “extra” de alto nivel, solo un cliente HTTP sencillo:

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<MiModelo> obtenerDato(String id) async {
  final uri = Uri.parse('https://api.ejemplo.com/items/$id');
  final response = await http.get(uri);
  if (response.statusCode == 200) {
    return MiModelo.fromJson(jsonDecode(response.body) as Map<String, dynamic>);
  }
  throw Exception('Error: ${response.statusCode}');
}

Future<void> enviarDato(MiModelo dato) async {
  final response = await http.post(
    Uri.parse('https://api.ejemplo.com/items'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode(dato.toJson()),
  );
  if (response.statusCode != 201) throw Exception('Error: ${response.statusCode}');
}
```

## Con HttpClient de dart:io

Si no quieres añadir dependencias, puedes usar `dart:io` (solo válido en plataformas nativas, no en web):

```dart
import 'dart:io';
import 'dart:convert';

Future<String> getRaw(String url) async {
  final client = HttpClient();
  try {
    final request = await client.getUrl(Uri.parse(url));
    final response = await request.close();
    return await response.transform(utf8.decoder).join();
  } finally {
    client.close();
  }
}
```

En web no existe `dart:io`; ahí se usa `http` (que usa el cliente del navegador) o APIs fetch. Para producción en móvil/desktop, `http` es la opción más simple y suficiente para la mayoría de APIs REST.

## Con librerías (dio y otras)

### dio

**dio** es un cliente HTTP con más capacidades: **BaseOptions** para baseUrl, connectTimeout, receiveTimeout y headers por defecto; **interceptors** para añadir tokens, logging o transformar request/response; cancelación de peticiones; errores unificados en **DioException** (tipo, statusCode, response). Ejemplo equivalente al anterior:

```dart
import 'package:dio/dio.dart';
import 'dart:convert';

final dio = Dio(BaseOptions(
  baseUrl: 'https://api.ejemplo.com',
  connectTimeout: Duration(seconds: 5),
  headers: {'Content-Type': 'application/json'},
));

Future<MiModelo> obtenerDato(String id) async {
  final response = await dio.get('/items/$id');
  if (response.statusCode == 200) {
    return MiModelo.fromJson(response.data as Map<String, dynamic>);
  }
  throw DioException(requestOptions: response.requestOptions, response: response);
}

Future<void> enviarDato(MiModelo dato) async {
  await dio.post('/items', data: dato.toJson());
}
```

**Ventajas frente a `http`:** Interceptors (auth, logging, reintentos), timeouts configurables por instancia o por petición, transformadores, y manejo de errores con `DioException.type` (connectTimeout, response, etc.). Para APIs grandes con muchos endpoints puede usarse **retrofit** (generación de cliente REST desde anotaciones) sobre dio.

### Cuándo usar qué

- **http:** Proyectos pequeños, mínimo de dependencias, pocos endpoints y sin necesidad de interceptors.
- **dio:** Proyectos con autenticación (token en header), varios dominios o endpoints, necesidad de interceptors, timeouts o manejo de errores más rico.

---

[← Volver al README principal](../../README.md)
