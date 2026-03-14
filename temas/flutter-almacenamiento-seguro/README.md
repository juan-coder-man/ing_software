# Flutter: almacenamiento seguro

Almacenar tokens, contraseñas o datos sensibles en Flutter debe hacerse de forma segura: usando **flutter_secure_storage**, que se apoya en Keychain (iOS) y Keystore/EncryptedSharedPreferences (Android), en lugar de guardar en claro en SharedPreferences o archivos.

## flutter_secure_storage

El paquete **flutter_secure_storage** guarda pares clave-valor cifrados en la plataforma nativa:

```dart
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

final storage = FlutterSecureStorage(
  aOptions: AndroidOptions(encryptedSharedPreferences: true),
);

await storage.write(key: 'token', value: 'jwt...');
String? token = await storage.read(key: 'token');
await storage.delete(key: 'token');
await storage.deleteAll();
```

En iOS usa Keychain; en Android puede usar Keystore o EncryptedSharedPreferences según configuración. No usar para grandes volúmenes; está pensado para secretos y datos pequeños.

## Qué no guardar en claro

- **Tokens de sesión (JWT, refresh token):** Usar secure storage; renovar y borrar al cerrar sesión.
- **Contraseñas:** No guardar la contraseña en texto plano; si se debe “recordar”, considerar solo token o credenciales del sistema (Keychain/Keystore).
- **API keys o secretos:** Preferir backend que exponga datos con auth; si la app debe llevar una key, usar secure storage y ofuscar en la medida posible (no es 100% seguro en dispositivo comprometido).

## Otras consideraciones

- **Backup:** En Android se puede desactivar el backup del contenido sensible (`android:allowBackup="false"` o excluir archivos sensibles).
- **Root/Jailbreak:** En dispositivos rooteados o con jailbreak el almacenamiento puede ser más vulnerable; algunas apps detectan root y limitan funcionalidad sensible.
- **Biometría:** Para desbloquear acceso a datos guardados se puede combinar secure storage con comprobación biométrica (local_auth) antes de leer.

---

[← Volver al README principal](../../README.md)
