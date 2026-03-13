# Principios SOLID

Los principios SOLID son cinco guías de diseño orientado a objetos para conseguir código más mantenible, flexible y comprensible.

## Resumen en tabla

| Letra | Nombre                    | En una frase                                                              | ¿Para qué sirve?                                                          |
| :---: | ------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **S** | Responsabilidad única     | Una clase, una sola tarea.                                                | Evitar que al cambiar una cosa se rompan otras sin querer.                |
| **O** | Abierto/Cerrado           | Añade comportamiento nuevo sin tocar el código que ya funciona.           | Poder extender el sistema sin miedo a romper lo existente.                |
| **L** | Sustitución de Liskov     | Lo que vale para el tipo base debe valer para el subtipo.                 | Que al usar herencia no aparezcan comportamientos raros o inesperados.    |
| **I** | Segregación de interfaces | Interfaces pequeñas y concretas, no una gigante con todo.                 | No obligar a implementar métodos que no se usan.                          |
| **D** | Inversión de dependencias | Depende de “contratos” (abstracciones), no de implementaciones concretas. | Poder cambiar piezas (BD, APIs, etc.) sin reescribir el resto del código. |

---

## S – Single Responsibility (Responsabilidad única)

Una clase debe tener una sola razón para cambiar: una única responsabilidad. Si una clase hace varias cosas, cualquier cambio en una de ellas puede romper las demás.

**Ejemplo (vida real):** Un restaurante separa quién cocina, quién sirve y quién cobra. Si el cocinero también cobrara y cambiaran las formas de pago, habría que tocar al cocinero y arriesgar errores en la cocina. Una persona (o módulo) = una responsabilidad.

**Ejemplo (código):**

```ts
// Mal: una clase hace demasiado
class Usuario {
  guardarEnBaseDeDatos() {
    /* ... */
  }
  enviarEmailBienvenida() {
    /* ... */
  }
  generarReportePDF() {
    /* ... */
  }
}

// Bien: cada clase una responsabilidad
class Usuario {
  /* solo datos y lógica de dominio */
}
class UsuarioRepositorio {
  guardar(usuario: Usuario) {
    /* ... */
  }
}
class EmailService {
  enviarBienvenida(email: string) {
    /* ... */
  }
}
class ReporteService {
  generarPDF(usuario: Usuario) {
    /* ... */
  }
}
```

---

## O – Open/Closed (Abierto/Cerrado)

Las entidades de software deben estar **abiertas a extensión** (nuevo comportamiento) pero **cerradas a modificación** (no cambiar código existente). Se extiende mediante herencia, composición o interfaces, no editando la clase original.

**Ejemplo (vida real):** Un enchufe está “cerrado” a cambios internos (no lo desarmas para cada aparato), pero “abierto” a extensión: enchufas lámpara, cargador o nevera sin modificar la pared. Añades comportamiento (nuevos dispositivos) sin tocar la instalación.

**Ejemplo (código):**

```ts
// Mal: cada tipo nuevo obliga a modificar la función
function area(forma: string, datos: number[]) {
  if (forma === "circulo") return Math.PI * datos[0] ** 2;
  if (forma === "rectangulo") return datos[0] * datos[1];
  // añadir más formas = modificar siempre este código
}

// Bien: extensión sin modificar
interface Forma {
  area(): number;
}
class Circulo implements Forma {
  constructor(private radio: number) {}
  area() {
    return Math.PI * this.radio ** 2;
  }
}
class Rectangulo implements Forma {
  constructor(
    private ancho: number,
    private alto: number,
  ) {}
  area() {
    return this.ancho * this.alto;
  }
}
// Nueva forma = nueva clase, sin tocar las demás
```

---

## L – Liskov Substitution (Sustitución de Liskov)

Los subtipos deben ser sustituibles por sus tipos base sin alterar el correcto funcionamiento del programa. Si una clase B hereda de A, cualquier uso de A debe poder reemplazarse por B sin que el cliente note la diferencia.

**Ejemplo (vida real):** Donde aceptas “un vehículo”, puedes poner un coche, una moto o un autobús. Quien usa “vehículo” espera arrancar, acelerar y frenar. Si un “vehículo” llamado patinete no pudiera llevar pasajeros como los demás sin romper las suposiciones, no sería un sustituto válido.

**Ejemplo (código):**

```ts
// Mal: el subtipo rompe lo que el cliente espera
class Rectangulo {
  setAncho(w: number) {
    this.ancho = w;
  }
  setAlto(h: number) {
    this.alto = h;
  }
  getArea() {
    return this.ancho * this.alto;
  }
}
class Cuadrado extends Rectangulo {
  setAncho(w: number) {
    this.ancho = this.alto = w;
  } // cambia también alto
  setAlto(h: number) {
    this.ancho = this.alto = h;
  } // el cliente no lo espera
}
// Quien hace setAncho(5); setAlto(4); espera area 20. Con Cuadrado da 16. No sustituye bien.

// Bien: subtipos que respetan el contrato
interface Figura {
  area(): number;
}
class Rectangulo implements Figura {
  constructor(
    private ancho: number,
    private alto: number,
  ) {}
  area() {
    return this.ancho * this.alto;
  }
}
class Cuadrado implements Figura {
  constructor(private lado: number) {}
  area() {
    return this.lado ** 2;
  }
}
// Quien usa Figura y llama area() obtiene el resultado correcto con cualquiera
```

---

## I – Interface Segregation (Segregación de interfaces)

Es mejor tener muchas interfaces pequeñas y específicas que una interfaz grande y genérica. Los clientes no deben depender de métodos que no usan.

**Ejemplo (vida real):** Un mando a distancia solo con botones de “encender” y “cambiar canal” está bien para ver la tele. Un mando con 50 botones de funciones que no usas (grabar, programar, menú técnico) te obliga a conocerlos aunque no los necesites. Mejor interfaces pequeñas por uso.

**Ejemplo (código):**

```ts
// Mal: interfaz gorda, el cliente depende de todo
interface Trabajador {
  trabajar(): void;
  comer(): void;
  dormir(): void;
  reuniones(): void;
}
class Robot implements Trabajador {
  trabajar() {
    /* ok */
  }
  comer() {
    throw new Error("Los robots no comen");
  } // obligado a implementar
  dormir() {
    throw new Error("Los robots no duermen");
  }
  reuniones() {
    /* ok */
  }
}

// Bien: interfaces segregadas
interface Trabajador {
  trabajar(): void;
}
interface SerVivo {
  comer(): void;
  dormir(): void;
}
interface Reunible {
  reuniones(): void;
}
class Robot implements Trabajador, Reunible {
  trabajar() {
    /* ... */
  }
  reuniones() {
    /* ... */
  }
}
class Persona implements Trabajador, SerVivo, Reunible {
  /* ... */
}
```

---

## D – Dependency Inversion (Inversión de dependencias)

Los módulos de alto nivel no deben depender de los de bajo nivel; ambos deben depender de abstracciones. Las abstracciones no deben depender de los detalles; los detalles deben depender de las abstracciones (inyección de dependencias, uso de interfaces).

**Ejemplo (vida real):** El conductor no depende del motor concreto (gasolina, eléctrico, híbrido): depende del “contrato” de “arrancar, acelerar, frenar”. El motor concreto se puede cambiar sin reescribir el manual del conductor. El nivel alto (conducir) depende de la abstracción, no del detalle (marca del motor).

**Ejemplo (código):**

```ts
// Mal: alto nivel atado al detalle
class Notificador {
  private gmail = new GmailService(); // acoplado a Gmail
  enviar(mensaje: string) {
    this.gmail.send(mensaje); // si cambias a SMS, cambias esta clase
  }
}

// Bien: ambos dependen de la abstracción
interface Mensajero {
  enviar(mensaje: string): void;
}
class Notificador {
  constructor(private mensajero: Mensajero) {}
  enviar(mensaje: string) {
    this.mensajero.enviar(mensaje);
  }
}
// Se inyecta GmailService, SmsService, etc., que implementan Mensajero
// Notificador no conoce el detalle, solo el contrato
```

---

[← Volver al README principal](../README.md)
