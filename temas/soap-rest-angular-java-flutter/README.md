# SOAP y REST en Angular, Java y Flutter

SOAP (XML sobre HTTP u otros protocolos) y REST (API HTTP con recursos y verbos) son estilos de integración con servicios. En Angular, Java y Flutter se usan librerías para consumir estos APIs; en muchos casos REST puede consumirse con clientes HTTP nativos sin librerías SOAP/REST específicas.

## SOAP vs REST (resumen)

| Aspecto     | SOAP                                | REST                                                |
| ----------- | ----------------------------------- | --------------------------------------------------- |
| Formato     | XML, WSDL                           | JSON (o XML), sin WSDL estándar                     |
| Protocolo   | HTTP, SMTP, etc.                    | HTTP principalmente                                 |
| Operaciones | Métodos definidos en WSDL           | Verbos HTTP (GET, POST, PUT, DELETE) sobre recursos |
| Uso típico  | Integraciones empresariales, legacy | APIs modernas, móviles, SPA                         |

## Consumir REST

### Angular

- **Con librería:** `HttpClient` (parte de Angular). Es el estándar; no es una librería externa.
- **Sin librería adicional:** `fetch()` del navegador o `HttpClient`; para REST no suele hacer falta otra cosa.

```ts
this.http.get<Dato[]>("/api/items").subscribe((data) => (this.items = data));
```

### Java

- **Con librería:** RestTemplate, WebClient (Spring), JAX-RS Client.
- **Sin librería específica REST:** `HttpURLConnection` o `HttpClient` (java.net.http, Java 11+) para hacer GET/POST y parsear JSON con Jackson o Gson.

```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder().uri(URI.create(url)).GET().build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
// Parsear response.body() con Jackson/Gson
```

### Flutter

- **Con librería:** `http` o `dio` (recomendado para producción).
- **Sin librería externa:** `HttpClient` de `dart:io` (o `http` que es muy ligera) para GET/POST y parsear JSON manualmente o con `dart:convert`.

```dart
final response = await http.get(Uri.parse(url));
if (response.statusCode == 200) {
  final data = jsonDecode(response.body);
}
```

## Consumir SOAP

SOAP suele requerir generar o usar clientes que entiendan WSDL y XML:

- **Angular:** No hay cliente SOAP nativo; se usa `HttpClient` para enviar XML manualmente o librerías como `soap` (npm).
- **Java:** JAX-WS, CXF o Spring WS para generar cliente desde WSDL o enviar peticiones SOAP.
- **Flutter:** No hay estándar; se envía XML con `http`/`dio` o se usa un backend intermedio que exponga REST y hable SOAP internamente.

En resumen: REST se puede consumir en los tres entornos con el cliente HTTP estándar (o el mínimo recomendado); SOAP suele implicar librerías o código que construya/parsee XML según WSDL.

---

[← Volver al README principal](../../README.md)
