# Extender vs Implementar

En Java, **extender** (`extends`) e **implementar** (`implements`) son los mecanismos para reutilizar o cumplir contratos. `extends` se usa con una sola clase (abstracta o concreta); `implements` se usa con una o más interfaces.

## Tabla resumen

| Palabra clave | Qué se pone a la derecha | Cantidad | Propósito                                         |
| ------------- | ------------------------ | -------- | ------------------------------------------------- |
| `extends`     | Una clase                | Una sola | Heredar estado y comportamiento de una superclase |
| `implements`  | Una o más interfaces     | Varias   | Cumplir el contrato de las interfaces             |

Una clase puede **extender** una clase y **implementar** varias interfaces a la vez:  
`class C extends B implements I1, I2 { }`

---

## Extender (extends)

Una clase **extiende** otra clase para heredar sus campos y métodos. La subclase puede sobrescribir métodos, añadir nuevos y usar `super` para invocar la superclase. En Java no hay herencia múltiple de clases: solo se puede extender **una** clase.

**Ejemplo (vida real):** “Perro” extiende “Animal”: hereda “respirar” y “comer” y añade “ladrar”.

**Ejemplo (código):**

```java
public class Animal {
    protected String nombre;
    public void respirar() { /* ... */ }
}

public class Perro extends Animal {
    public void ladrar() { /* ... */ }
    // nombre y respirar() heredados
}
```

---

## Implementar (implements)

Una clase **implementa** una o más interfaces y debe proporcionar cuerpo a todos los métodos abstractos de esas interfaces (o ser abstracta). No hereda estado de la interfaz; solo se compromete a ofrecer las operaciones definidas.

**Ejemplo (vida real):** Un “Reloj” puede implementar “Alarmable” y “Reproducible”: cumple dos contratos independientes sin ser “un tipo de” otra clase.

**Ejemplo (código):**

```java
public interface Alarmable {
    void activarAlarma();
}

public interface Reproducible {
    void play();
}

public class Reloj implements Alarmable, Reproducible {
    @Override
    public void activarAlarma() { /* ... */ }
    @Override
    public void play() { /* ... */ }
}
```

---

## Combinación extends + implements

Es habitual extender una clase base e implementar interfaces. La clase base aporta lógica común; las interfaces definen capacidades o contratos adicionales.

```java
public class MiServicio extends BaseService implements Logeable, Configurable {
    // Hereda de BaseService; cumple contrato de Logeable y Configurable
}
```

---

[← Volver al README principal](../../README.md)
