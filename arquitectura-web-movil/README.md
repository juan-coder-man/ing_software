# Arquitectura de Software Web y Móvil

La arquitectura de software para aplicaciones web y móviles organiza el sistema en capas y componentes con responsabilidades claras. Web y móvil comparten conceptos (cliente, API, negocio, datos) pero difieren en cómo se despliegan y consumen los servicios.

## Capas típicas

| Capa                       | Responsabilidad                           | Ejemplos                                                                          |
| -------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------- |
| **Presentación / Cliente** | UI e interacción con el usuario           | SPA (React, Angular), app nativa (Kotlin, Swift), híbrida (Flutter, React Native) |
| **API / Gateway**          | Punto de entrada, autenticación, enrutado | REST, GraphQL; API Gateway, BFF                                                   |
| **Lógica de negocio**      | Reglas y casos de uso                     | Servicios, dominio                                                                |
| **Acceso a datos**         | Persistencia y fuentes externas           | Repositorios, ORM, clientes de APIs externas                                      |
| **Infraestructura**        | Despliegue, mensajería, almacenamiento    | Contenedores, colas, almacenamiento en la nube                                    |

---

## Arquitectura web

En una aplicación web típica el navegador ejecuta el cliente (HTML/JS/CSS o una SPA). El servidor expone una API (REST o GraphQL) y aloja la lógica de negocio y el acceso a bases de datos. Puede haber una capa BFF (Backend for Frontend) que adapta la API al cliente.

**Ejemplo (vida real):** Una tienda online: el usuario usa la web (cliente); cada clic en “comprar” o “ver carrito” llama a la API del servidor; el servidor aplica reglas de negocio y lee/escribe en la base de datos.

**Flujo simplificado:**

- Cliente (navegador/SPA) → HTTP/HTTPS → Servidor (API + negocio + datos) → Base de datos / servicios externos.

---

## Arquitectura móvil

Las apps móviles pueden ser **nativas** (específicas de iOS/Android), **híbridas** (WebView + puente nativo) o **multiplataforma** (Flutter, React Native, etc.). También consumen APIs en el servidor; la diferencia está en el cliente: instalado en el dispositivo, con acceso a sensores y notificaciones push.

**Consideraciones:**

- **Conectividad:** diseño para redes inestables (caché, colas offline, reintentos).
- **Seguridad:** tokens, certificados, almacenamiento seguro de credenciales en el dispositivo.
- **API:** misma API para web y móvil (reutilización) o BFF específico para móvil si los contratos difieren.

---

## Arquitecturas concretas

### Enfocadas a software móvil

- **MVC (Model-View-Controller):** El modelo tiene datos y lógica; la vista muestra; el controlador recibe input y actualiza modelo y vista. Común en UIKit (iOS) y en Android antiguo; la vista suele estar muy acoplada al modelo.
- **MVP (Model-View-Presenter):** La vista es pasiva; el presentador maneja lógica y actualiza la vista mediante una interfaz. Mejora testabilidad y separación en Android.
- **MVVM (Model-View-ViewModel):** El ViewModel expone estado y comandos; la vista se enlaza por datos (data binding). Muy usada en Android (Jetpack) y en SwiftUI/Combine.
- **MVI (Model-View-Intent):** Flujo unidireccional: Intent (acción usuario) → Model (estado) → View. Todo el estado en un solo sitio; útil para UIs predecibles y depuración.
- **Clean Architecture / Hexagonal (en móvil):** Capas por dependencias (UI → casos de uso → entidades; adaptadores para frameworks). El dominio y la lógica de negocio no dependen del framework; en móvil suele combinarse con MVVM o MVI en la capa de presentación.
- **BFF (Backend for Frontend) por cliente:** Un backend específico para la app móvil que adapta APIs genéricas al contrato que el cliente necesita (menos round-trips, payloads ajustados), manteniendo una arquitectura de servidor distinta a la del cliente.

### Otras arquitecturas (servidor y sistema)

- **Monolito:** Una sola aplicación que despliega todas las capas (presentación, negocio, datos). Más simple de desplegar y desarrollar; puede modularizarse por paquetes o módulos (p. ej. JPMS).
- **Microservicios:** Servicios pequeños e independientes, cada uno con su despliegue y dominio. Comunicación por red (HTTP, mensajes). Escalado y evolución por servicio; añade complejidad operativa y de consistencia.
- **BFF (Backend for Frontend):** Un backend por tipo de cliente (web, móvil, etc.) que orquesta y adapta APIs internas; el front (web o móvil) solo habla con su BFF.
- **Capa API + negocio + datos (3 capas):** API/Gateway, lógica de negocio, acceso a datos; aplicable tanto a web como a móvil cuando el cliente consume la misma API.

---

## Web vs móvil (resumen)

| Aspecto            | Web                          | Móvil                                          |
| ------------------ | ---------------------------- | ---------------------------------------------- |
| Cliente            | Navegador, SPA o SSR         | App instalada (nativa/híbrida/multiplataforma) |
| Despliegue cliente | Servidor web, CDN            | Tiendas de aplicaciones, actualizaciones OTA   |
| Interacción        | Teclado, ratón, tacto        | Principalmente tacto, gestos, sensores         |
| Backend            | Común: API + negocio + datos | Mismo backend en muchos casos                  |

Una arquitectura bien definida separa claramente presentación, API, negocio y datos para que web y móvil puedan evolucionar y reutilizar la misma lógica de servidor.

---

[← Volver al README principal](../README.md)
