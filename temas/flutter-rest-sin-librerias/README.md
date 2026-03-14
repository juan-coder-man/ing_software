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

---

[← Volver al README principal](../../README.md)
