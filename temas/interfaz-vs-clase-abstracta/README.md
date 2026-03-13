# Interfaz vs Clase Abstracta

En Java, tanto las **interfaces** como las **clases abstractas** permiten definir contratos y comportamientos compartidos. La elección entre una y otra afecta la herencia, el acoplamiento y la evolución del diseño.

## Tabla comparativa

| Aspecto        | Interfaz                                                     | Clase abstracta                                   |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| Herencia       | Una clase puede implementar varias interfaces                | Una clase solo puede extender una clase abstracta |
| Implementación | Métodos por defecto opcionales (default/static desde Java 8) | Puede tener métodos con cuerpo y campos           |
| Estado         | No tiene campos de instancia (solo constantes estáticas)     | Puede tener atributos y estado                    |
| Constructores  | No tiene                                                     | Sí tiene (para subclases)                         |
| Modificadores  | Métodos públicos (o package-private en interfaces)           | Cualquier visibilidad en métodos y campos         |
| Uso típico     | Contrato / capacidad / rol                                   | Base común con lógica y estado compartido         |

---

## Interfaz

Define un **contrato**: qué operaciones debe ofrecer quien la implemente. No tiene estado; desde Java 8 puede tener métodos `default` y `static` con implementación. Permite **múltiple herencia de tipo**: una clase puede implementar varias interfaces.

**Ejemplo (vida real):** Un contrato de “reproducible”: cualquier reproductor (móvil, TV, consola) que implemente “play/pausa/stop” cumple el contrato, sin importar la tecnología interna.

**Ejemplo (código):**

```java
public interface Reproducible {
    void play();
    void pausar();
    void detener();
}

public class ReproductorMusica implements Reproducible {
    @Override
    public void play() { /* ... */ }
    @Override
    public void pausar() { /* ... */ }
    @Override
    public void detener() { /* ... */ }
}
```

---

## Clase abstracta

Representa una **base común** con estado y/o lógica parcial. Puede tener campos, constructores y métodos con implementación. Una clase concreta solo puede extender **una** clase abstracta. Útil cuando varias clases comparten estructura y comportamiento que quieres centralizar.

**Ejemplo (vida real):** “Vehículo” como clase abstracta: tiene campos (combustible, velocidad) y métodos comunes (arrancar, frenar); “Coche” y “Moto” extienden y completan el comportamiento.

**Ejemplo (código):**

```java
public abstract class Vehiculo {
    protected double combustible;
    protected boolean encendido;

    public void arrancar() {
        if (combustible > 0) encendido = true;
    }

    public abstract void acelerar();
}

public class Coche extends Vehiculo {
    @Override
    public void acelerar() { /* ... */ }
}
```

---

## Cuándo usar cada una

- **Interfaz:** cuando defines un contrato o capacidad que pueden tener tipos no relacionados (múltiples implementaciones, polimorfismo por capacidad). Preferible para dependencias y pruebas (inyección de implementaciones).
- **Clase abstracta:** cuando hay una jerarquía clara y quieres compartir estado y lógica entre subclases que son “un tipo de” la base.

---

[← Volver al README principal](../README.md)
