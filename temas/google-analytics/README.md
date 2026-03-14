# Google Analytics

Google Analytics (GA) es un servicio de medición y análisis de uso de sitios web y aplicaciones. La versión actual **GA4** (Google Analytics 4) se centra en eventos y medición basada en usuario, con integración en web (gtag, Google Tag Manager) y en apps móviles (SDK).

## Conceptos GA4

| Concepto       | Descripción                                                                                         |
| -------------- | --------------------------------------------------------------------------------------------------- |
| **Propiedad**  | Recurso que agrupa los datos de un sitio o app (GA4 reemplaza las “vistas” de Universal Analytics). |
| **Eventos**    | Acciones que se registran: page_view, scroll, clic, compra, evento personalizado.                   |
| **Parámetros** | Datos asociados a un evento (p. ej. valor, categoría, ID de ítem).                                  |
| **Métricas**   | Medidas agregadas: usuarios, sesiones, eventos por sesión, ingresos, etc.                           |
| **Informes**   | Exploraciones, informes estándar (adquisición, compromiso, monetización) y reportes personalizados. |

## Integración en web

- **gtag.js:** Cargar el script de Google con el ID de medición (G-XXXXXX); llamar `gtag('event', 'nombre_evento', { parametros })` para eventos.
- **Google Tag Manager (GTM):** Gestionar tags (GA4, otros) sin tocar código; disparar eventos según reglas (clics, formularios, variables). Útil cuando marketing o no desarrolladores gestionan la medición.

## Integración en apps (Android/iOS)

- **Firebase Analytics + GA4:** En apps que usan Firebase, los eventos de Analytics se enlazan a la propiedad GA4; el SDK envía eventos automáticos y personalizados.
- **SDK nativo GA4:** Para apps sin Firebase se puede usar el SDK de Google Analytics para la plataforma correspondiente y enviar los mismos tipos de eventos.

## Privacidad y buenas prácticas

- **Consentimiento:** Respetar preferencias de usuario (RGPD, CCPA); no enviar datos de medición hasta que se dé el consentimiento; configurar modo de consentimiento en gtag/GTM si aplica.
- **Datos sensibles:** No enviar PII (emails, nombres completos) en eventos o parámetros; usar IDs anónimos o hashes si es necesario para análisis.
- **Objetivos claros:** Definir eventos clave (conversiones) y parámetros necesarios; evitar disparar demasiados eventos que no se usen en informes.

---

[← Volver al README principal](../../README.md)
