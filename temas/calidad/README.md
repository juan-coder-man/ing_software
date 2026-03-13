# Atributos de calidad del software

Los atributos de calidad son características del diseño y del código que mejoran la calidad interna del software: facilitan el mantenimiento, la evolución y el trabajo en equipo sin alterar lo que el usuario percibe directamente.

## Tabla resumen

| Atributo          | En una frase                                                          | Relación con el diseño                                        |
| ----------------- | --------------------------------------------------------------------- | ------------------------------------------------------------- |
| Mantenibilidad    | Facilidad para corregir errores y adaptar el sistema con seguridad    | Código modular y con responsabilidades claras                 |
| Escalabilidad     | Capacidad de crecer en carga, datos o funcionalidad sin rehacerlo     | Arquitectura que permite añadir piezas sin romper las demás   |
| Comprensibilidad  | Código legible y con intención clara para cualquier desarrollador     | Nombres claros, funciones cortas, poca complejidad accidental |
| Bajo acoplamiento | Módulos con pocas dependencias entre sí; cambios aislados             | Depender de abstracciones, no de implementaciones concretas   |
| Testabilidad      | Facilidad para escribir y ejecutar pruebas unitarias o de integración | Dependencias inyectables, responsabilidades únicas            |

---

## Mantenibilidad

Facilidad para corregir defectos y adaptar el sistema a nuevos requisitos sin introducir efectos secundarios ni miedo a romper otras partes.

**Ejemplo (vida real):** Un coche mantenible permite cambiar pastillas de freno o filtros sin desmontar medio motor. Cada pieza tiene un rol claro y se reemplaza de forma predecible.

**Ejemplo (código):** Una clase con una sola responsabilidad se mantiene mejor que una que mezcla persistencia, notificaciones y reportes.

```ts
// Mantenible: cambiar la forma de guardar solo afecta a UsuarioRepositorio
class UsuarioRepositorio {
  guardar(usuario: Usuario): void {
    // Un solo lugar para cambios en persistencia
  }
}
```

---

## Escalabilidad

Capacidad del sistema de crecer: más usuarios, más datos o más funcionalidad, sin tener que reescribir todo. Puede ser escalabilidad de rendimiento (más carga) o de diseño (más características).

**Ejemplo (vida real):** Una tienda que puede abrir más cajas o más sucursales sin cambiar la forma de cobrar o de gestionar inventario escala mejor que una donde cada ampliación obliga a rehacer procesos.

**Ejemplo (código):** Añadir un nuevo tipo de notificación (SMS, push) sin tocar el núcleo del negocio.

```ts
// Escalable: nuevo canal = nueva clase que implementa la interfaz
interface CanalNotificacion {
  enviar(mensaje: string, destino: string): void;
}
// Se añaden SmsCanal, PushCanal, etc. sin modificar el orquestador
```

---

## Comprensibilidad

El código es fácil de leer y de entender: nombres que reflejan la intención, funciones cortas, poca complejidad innecesaria. Cualquier desarrollador puede seguir la lógica sin adivinar.

**Ejemplo (vida real):** Un manual de instrucciones con pasos numerados y términos claros se entiende mejor que un párrafo denso con jerga.

**Ejemplo (código):** Nombres y estructura que explican el qué y el porqué.

```ts
// Comprensible: se entiende la intención sin comentarios
function calcularPrecioConDescuento(
  precioBase: number,
  porcentajeDescuento: number,
): number {
  const factorDescuento = 1 - porcentajeDescuento / 100;
  return precioBase * factorDescuento;
}
```

---

## Bajo acoplamiento

Los módulos o clases dependen poco entre sí; un cambio en uno no obliga a tocar muchos otros. Se logra dependiendo de abstracciones (interfaces, contratos) y no de implementaciones concretas.

**Ejemplo (vida real):** El enchufe de la pared no depende del aparato concreto; cualquier dispositivo que cumpla el contrato (voltaje, forma) funciona. Bajo acoplamiento entre red eléctrica y dispositivos.

**Ejemplo (código):** El orquestador depende de una interfaz, no de una base de datos o API concreta.

```ts
// Bajo acoplamiento: depende de la abstracción
interface AlmacenUsuarios {
  buscarPorId(id: string): Usuario | null;
}
class ServicioPedidos {
  constructor(private usuarios: AlmacenUsuarios) {}
  // No sabe si es MySQL, API REST o memoria; solo usa el contrato
}
```

---

## Testabilidad

El diseño permite escribir pruebas unitarias o de integración de forma sencilla: dependencias inyectables, responsabilidades claras y poco estado oculto. Se puede aislar la lógica a probar y sustituir colaboradores por dobles (mocks).

**Ejemplo (vida real):** Un equipo médico puede probar un nuevo protocolo en un simulador antes de usarlo con pacientes. Las condiciones son controlables y repetibles.

**Ejemplo (código):** La lógica de negocio recibe sus dependencias por constructor; en tests se inyectan mocks.

```ts
// Testeable: se inyecta un doble que no envía emails reales
class Notificador {
  constructor(private mensajero: { enviar(texto: string): void }) {}
  notificarBienvenida(email: string): void {
    this.mensajero.enviar(`Bienvenido, ${email}`);
  }
}
// En test: mensajero = { enviar: jest.fn() }
```

---

[← Volver al README principal](../README.md)
