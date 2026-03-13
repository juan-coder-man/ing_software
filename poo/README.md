# Programación orientada a objetos (POO)

La POO es un paradigma de programación basado en el concepto de **objetos**, que encapsulan datos (atributos) y comportamiento (métodos), y en las relaciones entre ellos.

## Tabla comparativa de los pilares

| Pilar           | En una frase                                           | Analogía rápida                                             |
| --------------- | ------------------------------------------------------ | ----------------------------------------------------------- |
| Encapsulamiento | Ocultar detalles internos, exponer lo necesario        | Caja negra: solo usas el panel de control, no el interior   |
| Herencia        | Reutilizar y extender de una clase base                | Plantillas que se especializan (ej. vehículo → coche, moto) |
| Polimorfismo    | Mismo mensaje, comportamientos distintos según el tipo | Un mismo botón hace cosas distintas en cada aplicación      |
| Abstracción     | Contrato: qué hace, no cómo                            | Menú del restaurante: eliges el plato, no ves la cocina     |

## Clases y objetos

- **Clase:** plantilla o molde que define atributos y métodos comunes.
- **Objeto:** instancia concreta de una clase, con estado propio.

## Pilares principales

### Encapsulamiento

Ocultar los detalles internos y exponer solo lo necesario. Los atributos suelen ser privados y se accede mediante métodos (getters/setters o métodos de dominio), protegiendo la integridad del estado.

**Ejemplo de la vida real:** Cuenta bancaria. No ves el saldo interno ni cómo se calcula; solo usas "depositar", "retirar" y "consultar saldo". El banco protege los datos y expone solo operaciones válidas.

**Ejemplo técnico:**

```typescript
class CuentaBancaria {
  private saldo: number = 0;

  depositar(monto: number): void {
    if (monto > 0) this.saldo += monto;
  }

  retirar(monto: number): boolean {
    if (monto > 0 && monto <= this.saldo) {
      this.saldo -= monto;
      return true;
    }
    return false;
  }

  consultarSaldo(): number {
    return this.saldo;
  }
}
```

### Herencia

Una clase puede heredar atributos y métodos de otra (superclase o clase base), permitiendo reutilizar código y establecer una jerarquía de tipos. La subclase puede extender o sobrescribir el comportamiento.

**Ejemplo de la vida real:** Vehículo → Coche, Moto, Camión. Todos tienen "arrancar" y "frenar", pero cada uno implementa detalles distintos (ruedas, marchas, capacidad).

**Ejemplo técnico:**

```typescript
class Vehiculo {
  protected encendido: boolean = false;

  arrancar(): void {
    this.encendido = true;
  }

  frenar(): void {
    // Comportamiento base
  }
}

class Coche extends Vehiculo {
  private marcha: number = 0;

  arrancar(): void {
    super.arrancar();
    this.marcha = 1;
  }
}

class Moto extends Vehiculo {
  arrancar(): void {
    super.arrancar();
    // Lógica específica de moto
  }
}
```

### Polimorfismo

Objetos de diferentes tipos pueden responder al mismo mensaje o interfaz. Permite tratar instancias de subclases como si fueran de la clase base; el comportamiento concreto depende del tipo real en tiempo de ejecución.

**Ejemplo de la vida real:** Botón "Reproducir": en Spotify reproduce música, en YouTube un vídeo, en Netflix una película. Mismo nombre de acción, comportamiento distinto según la aplicación.

**Ejemplo técnico:**

```typescript
interface Reproductor {
  reproducir(): void;
}

class SpotifyReproductor implements Reproductor {
  reproducir(): void {
    // Reproduce música
  }
}

class YoutubeReproductor implements Reproductor {
  reproducir(): void {
    // Reproduce vídeo
  }
}

function usarReproductor(r: Reproductor): void {
  r.reproducir(); // Comportamiento distinto según el tipo real
}

usarReproductor(new SpotifyReproductor());
usarReproductor(new YoutubeReproductor());
```

### Abstracción

Enfocarse en qué hace un objeto (contrato, interfaz) y no en cómo lo hace internamente. Las clases abstractas e interfaces son herramientas típicas para definir abstracciones.

**Ejemplo de la vida real:** Encender la luz. Solo te importa "encender" o "apagar"; no necesitas saber si es LED, incandescente o cómo está cableado. El contrato es el interruptor; la implementación queda oculta.

**Ejemplo técnico:**

```typescript
interface Interruptor {
  encender(): void;
  apagar(): void;
}

class LuzLED implements Interruptor {
  encender(): void {
    // Encender circuito LED
  }
  apagar(): void {
    // Apagar circuito LED
  }
}

class LuzIncandescente implements Interruptor {
  encender(): void {
    // Pasar corriente al filamento
  }
  apagar(): void {
    // Cortar corriente
  }
}

// El código que usa el interruptor no conoce el tipo de bombilla
function toggle(luz: Interruptor, activo: boolean): void {
  activo ? luz.encender() : luz.apagar();
}
```

---

[← Volver al README principal](../README.md)
