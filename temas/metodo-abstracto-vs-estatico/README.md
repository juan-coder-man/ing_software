# Método Abstracto vs Método Estático

Los **métodos abstractos** obligan a las subclases a dar una implementación concreta y forman parte del polimorfismo. Los **métodos estáticos** pertenecen a la clase, no se sobrescriben y se invocan sin instancia. Son conceptos distintos y con roles diferentes en el diseño.

## Tabla comparativa

| Aspecto         | Método abstracto                                  | Método estático                                      |
| --------------- | ------------------------------------------------- | ---------------------------------------------------- |
| Dónde se define | Clase abstracta o interfaz                        | Clase (o interfaz desde Java 8)                      |
| Implementación  | Sin cuerpo en la clase base; la dan las subclases | Con cuerpo en la clase que lo define                 |
| Sobrescritura   | Sí: cada subclase implementa a su manera          | No: no hay polimorfismo por instancia                |
| Invocación      | Sobre un objeto (instancia)                       | Sobre la clase: `Clase.metodo()`                     |
| Estado          | Puede usar estado de la instancia                 | No accede a instancia; solo a parámetros y estáticos |
| Uso típico      | Contrato polimórfico                              | Utilidades, factories, constantes de cálculo         |

---

## Método abstracto

Declara una operación sin implementación. Cualquier clase concreta que extienda la clase abstracta (o implemente la interfaz) debe implementar ese método. Es la base del polimorfismo: la misma llamada puede ejecutar comportamientos distintos según el tipo real del objeto.

**Ejemplo (vida real):** “Calcular área” en “Figura” es abstracto: cada figura (círculo, rectángulo) tiene su propia fórmula.

**Ejemplo (código):**

```java
public abstract class Figura {
    public abstract double area();
}

public class Circulo extends Figura {
    private double radio;
    @Override
    public double area() {
        return Math.PI * radio * radio;
    }
}

// Uso polimórfico
Figura f = new Circulo(5);
double a = f.area(); // se ejecuta Circulo.area()
```

---

## Método estático

Pertenece a la clase, no a la instancia. Se llama con el nombre de la clase (o desde dentro de la clase sin objeto). No puede ser abstracto en una clase concreta (no tiene sentido: no se sobrescribe). No accede a `this`.

**Ejemplo (vida real):** “Convertir kilómetros a millas” no depende de un objeto concreto; es una función de utilidad asociada al concepto “Conversor”.

**Ejemplo (código):**

```java
public class MathUtils {
    public static double kmAMillas(double km) {
        return km * 0.621371;
    }
}

double millas = MathUtils.kmAMillas(10); // sin crear instancia
```

**Importante:** un método estático en una superclase no se sobrescribe en la subclase; si la subclase define un método con la misma firma y `static`, solo oculta el de la superclase, sin polimorfismo.

---

[← Volver al README principal](../README.md)
