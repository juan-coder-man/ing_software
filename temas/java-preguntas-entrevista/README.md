# Preguntas de entrevista Java

Preguntas frecuentes de entrevistas técnicas sobre Java, basadas en el repositorio [Devinterview-io/java-interview-questions](https://github.com/Devinterview-io/java-interview-questions). El README de ese repo incluye 15 preguntas con respuestas completas; las 100 preguntas están disponibles en [Devinterview.io - Java](https://devinterview.io/questions/web-and-mobile-development/java-interview-questions).

---

## 1. Explica la idea principal de _Java_ y el concepto de _Write Once, Run Anywhere_

**Java** es un lenguaje de programación de alto nivel y orientado a objetos diseñado para ser **independiente de la plataforma**. Su filosofía central se resume en "**Write Once, Run Anywhere**" (WORA), que permite que el mismo código se ejecute en distintos sistemas sin modificarlo.

### Write Once, Run Anywhere (WORA)

WORA permite escribir el código una vez y ejecutarlo en cualquier dispositivo o sistema operativo. Esto se logra con:

#### 1. Java Virtual Machine (JVM)

La **JVM** actúa como capa de abstracción entre el código Java y el hardware. Interpreta y ejecuta el bytecode, garantizando un comportamiento uniforme en distintas plataformas.

#### 2. Bytecode

El código fuente se compila a **bytecode** independiente de la plataforma, ejecutable por cualquier JVM.

#### 3. Biblioteca estándar

Java ofrece una biblioteca estándar con capacidades multiplataforma para E/S, red, etc.

### Cómo Java logra WORA

1. **Independencia de plataforma**: el bytecode corre en cualquier dispositivo con una JVM compatible.
2. **JVM por plataforma**: cada sistema operativo tiene su implementación de la JVM.
3. **Recolección de basura**: gestión automática de memoria.
4. **Seguridad**: sin manipulación directa de memoria mediante punteros.

### Ejemplo: "Hello, World!" en Java

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Se compila y ejecuta en cualquier sistema con JVM:

1. Compilar: `javac HelloWorld.java`
2. Ejecutar: `java HelloWorld`
3. Salida: `Hello, World!`

---

## 2. ¿Cuáles son las _principales características_ de _Java_?

### Características principales

1. **Independencia de plataforma**: WORA mediante la JVM.
2. **Orientado a objetos**: encapsulación, herencia y polimorfismo.
3. **Tipado fuerte**: reduce ambigüedad y errores.
4. **Seguridad**: verificador de bytecode, security manager.
5. **Gestión automática de memoria**: garbage collection.
6. **Concurrencia**: multihilo.
7. **Neutral en arquitectura**: escalable en distintos entornos.
8. **Dinámico**: carga dinámica de clases y compilación dinámica.
9. **Simplicidad**: sintaxis y bibliotecas estándar.
10. **Portabilidad**: "compile once, run anywhere".
11. **Rendimiento**: compilación JIT (Just-In-Time).

### Otras características

#### Manejo de excepciones

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("No se puede dividir por cero");
}
```

#### Biblioteca estándar

```java
import java.util.ArrayList;
import java.util.List;

List<String> list = new ArrayList<>();
list.add("Java");
list.add("es");
list.add("potente");
```

#### Concurrencia

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> {
    System.out.println("Tarea ejecutada por " + Thread.currentThread().getName());
});
```

---

## 3. ¿Qué características _no orientadas a objetos_ tiene _Java_?

Java incluye elementos que no son puramente OOP:

### Tipos primitivos

`int`, `boolean`, `char`, etc., no son objetos.

```java
int number = 42;
boolean isTrue = true;
char letter = 'A';
```

### Métodos y variables estáticas

Pertenecen a la clase, no a las instancias.

```java
public class MathUtils {
    public static final double PI = 3.14159;

    public static int add(int a, int b) {
        return a + b;
    }
}
```

### Acceso a nivel de paquete

El modificador por defecto (package-private) limita la visibilidad al mismo paquete.

```java
package com.example;

class PackagePrivateClass {
    void packagePrivateMethod() {
        // Solo accesible dentro del mismo paquete
    }
}
```

### Clases de utilidad

Clases con solo métodos estáticos, sin instanciación.

```java
public final class StringUtils {
    private StringUtils() {}

    public static boolean isEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }
}
```

### Herencia simple

Solo una superclase por clase.

```java
public class Animal {}
public class Mammal extends Animal {}
```

### Estilo procedural

Dentro de métodos (p. ej. `main`) se puede escribir de forma procedural.

```java
public static void main(String[] args) {
    int x = 5;
    int y = 10;
    int sum = x + y;
    System.out.println("Suma: " + sum);
}
```

---

## 4. Diferencia entre _JDK_, _JRE_ y _JVM_

La **JVM** (Java Virtual Machine) es la máquina abstracta que ejecuta el bytecode. Es el núcleo de "write once, run anywhere".

#### Funciones de la JVM

- **Interpretación de bytecode**: traduce bytecode a instrucciones de máquina.
- **Gestión de memoria**: asignación, liberación y garbage collection.
- **JIT**: compila tramos frecuentes a código nativo.
- **Excepciones**: ejecuta bloques try-catch.
- **Seguridad**: modelo de seguridad de Java.

### JRE (Java Runtime Environment)

Entorno mínimo para **ejecutar** una aplicación Java: JVM + bibliotecas básicas y archivos de soporte.

- JVM para la plataforma concreta.
- Bibliotecas (p. ej. `java.lang`, `java.util`).
- Archivos de configuración y recursos.

### JDK (Java Development Kit)

Incluye el JRE y todo lo necesario para **desarrollar**:

- **JRE** completo.
- **Herramientas**: `javac`, `java`, `javadoc`, `jdb`.
- **Bibliotecas adicionales** (p. ej. `javax`).

### Uso

- **Desarrollo**: JDK (compilar, depurar).
- **Despliegue**: JRE en máquinas de usuarios.
- **Ejecución**: la JVM (dentro del JRE/JDK) ejecuta el programa.

### Ejemplo

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

1. Compilar con `javac` (JDK): `javac HelloWorld.java` → genera `HelloWorld.class`.
2. Ejecutar con `java` (JRE): `java HelloWorld` → la JVM ejecuta el bytecode.

---

## 5. ¿Cuál es el papel del _ClassLoader_?

El **ClassLoader** carga los archivos de clase (`.class`) en memoria.

### Funciones

1. **Carga**: busca y lee la representación binaria de la clase.
2. **Enlace**:
   - **Verificación**: que la clase cumpla la especificación Java/JVM.
   - **Preparación**: memoria para variables de clase y valores por defecto.
   - **Resolución**: referencias simbólicas a referencias directas.
3. **Inicialización**: ejecuta inicializadores estáticos y campos estáticos.

### Tipos de ClassLoaders

1. **Bootstrap**: código nativo; carga clases core (p. ej. desde módulos en Java 9+).
2. **Extension/Platform** (Java 9+): directorio `lib/ext` o `java.ext.dirs`.
3. **Application**: clases del classpath definidas por el usuario.
4. **Custom**: loaders definidos por el usuario.

### Delegación

El ClassLoader delega primero en el padre; si el padre no puede cargar la clase, intenta cargarla el actual, hasta el Bootstrap.

### Carga dinámica

- `Class.forName(String className)`
- `ClassLoader.loadClass(String name)`

Útil para sistemas de plugins.

### Ejemplo

```java
public class ClassLoaderDemo {
    public static void main(String[] args) throws Exception {
        Class<?> stringClass = Class.forName("java.lang.String");
        System.out.println("Cargada: " + stringClass.getName());

        ClassLoader classLoader = ClassLoaderDemo.class.getClassLoader();
        Class<?> mathClass = classLoader.loadClass("java.lang.Math");
        System.out.println("Cargada: " + mathClass.getName());

        ClassLoader current = ClassLoaderDemo.class.getClassLoader();
        while (current != null) {
            System.out.println(current.getClass().getName());
            current = current.getParent();
        }
        System.out.println("Bootstrap ClassLoader");
    }
}
```

---

## 6. Diferencia entre _path_ y _classpath_ en _Java_

### Classpath

Indica a la **JVM** dónde buscar clases compiladas (`.class`) y JARs en tiempo de ejecución.

- Es específico del entorno Java.
- Puede incluir directorios, JAR y ZIP.
- Se usa con `-cp` o `-classpath`, o la variable `CLASSPATH`. Maven/Gradle lo gestionan automáticamente.

```bash
java -cp .:/ruta/a/algun.jar MiApp
export CLASSPATH=.:/ruta/a/algun.jar
```

### Path

Variable del **sistema operativo** que indica dónde están los ejecutables. No es específica de Java.

- Solo directorios.
- El SO la usa para resolver comandos.

```bash
export PATH=$PATH:/nuevo/directorio
```

### Comparación

| Aspecto   | Classpath                 | Path                  |
| --------- | ------------------------- | --------------------- |
| Propósito | Localizar clases Java     | Localizar ejecutables |
| Ámbito    | Entorno de ejecución Java | Sistema operativo     |
| Contenido | Directorios, JAR, ZIP     | Solo directorios      |
| Lo usa    | JVM                       | SO                    |

### Problemas habituales

- **ClassNotFoundException**: classpath incompleto.
- **NoClassDefFoundError**: clase no encontrada en tiempo de ejecución.
- **Conflictos de versiones**: varias versiones de una clase en el classpath.

---

## 7. Diferencia entre _int_ e _Integer_ en _Java_

### int

- **Tipo primitivo**.
- Enteros entre −2³¹ y 2³¹ − 1.
- 32 bits (4 bytes).
- Valor por defecto: `0`.
- Más rápido; no se puede usar en genéricos.

### Integer

- **Clase envolvente** del primitivo `int`.
- Métodos de utilidad; puede ser `null`.
- Más memoria (objeto).
- Se puede usar en genéricos (p. ej. `List<Integer>`).

### Autoboxing y unboxing

```java
int primitiveInt = 10;
Integer objInt = Integer.valueOf(20);

Integer autoBoxed = primitiveInt;  // autoboxing
int unboxed = objInt;               // unboxing
```

---

## 8. ¿Qué son las _wrapper classes_ en _Java_?

Permiten tratar tipos primitivos como objetos. Son necesarias en colecciones genéricas y en Java Beans.

### Tabla de wrappers

| Primitivo | Wrapper   | Métodos típicos                           |
| --------- | --------- | ----------------------------------------- |
| boolean   | Boolean   | valueOf(), parseBoolean(), booleanValue() |
| byte      | Byte      | valueOf(), parseByte(), byteValue()       |
| char      | Character | valueOf(), charValue()                    |
| short     | Short     | valueOf(), parseShort(), shortValue()     |
| int       | Integer   | valueOf(), parseInt(), intValue()         |
| long      | Long      | valueOf(), parseLong(), longValue()       |
| float     | Float     | valueOf(), parseFloat(), floatValue()     |
| double    | Double    | valueOf(), parseDouble(), doubleValue()   |

### Uso en colecciones y nullabilidad

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(5);
int num = numbers.get(0);

Integer age = null;  // válido
// int primitiveAge = null;  // error de compilación
```

---

## 9. ¿Qué significa que _Java_ sea un lenguaje _tipado estáticamente_?

El **tipo** de cada variable se conoce en **tiempo de compilación**. Hay que declarar el tipo antes de usar la variable.

### Ventajas

- **Seguridad de tipos**: menos errores en tiempo de ejecución.
- **Rendimiento**: optimizaciones en compilación.
- **Previsibilidad** y mejor soporte en el IDE (autocompletado, detección de errores).

### Ejemplo

```java
int number = 10;
String text = "Hello, Java!";
double decimal = 3.14;

int sum = number + 5;
// int result = number + text;  // error de compilación

double convertedNumber = (double) number;
```

---

## 10. ¿_Java_ es un lenguaje _puramente orientado a objetos_? ¿Por qué?

**No.** Aunque aplica encapsulación, abstracción, herencia y polimorfismo, incluye elementos no OOP:

1. **Tipos primitivos**: `int`, `boolean`, `char`, etc., no son objetos.
2. **Miembros estáticos**: campos y métodos de clase, no de instancia.
3. **Construcciones procedurales**: `if`, `for`, `while`, etc.

```java
public class Example {
    private int instanceVar;
    public static int staticVar = 10;

    public void instanceMethod() {
        if (instanceVar > 5) {
            System.out.println("Mayor que 5");
        }
    }

    public static void main(String[] args) {
        int localVar = 20;
        Example obj = new Example();
        obj.instanceMethod();
        System.out.println(Example.staticVar);
    }
}
```

---

## 11. ¿Qué es el _bytecode_ en _Java_?

Es la representación **intermedia** que genera el compilador Java: instrucciones compactas e independientes de la plataforma que ejecuta la JVM.

### Características

1. **Independencia de plataforma**: corre en cualquier JVM compatible.
2. **Formato compacto**: más breve que el código de máquina equivalente.
3. **Verificación**: la JVM verifica el bytecode antes de ejecutarlo.

### Flujo

1. **Compilación**: `javac MiPrograma.java` → `MiPrograma.class`.
2. **Interpretación**: la JVM interpreta el bytecode.
3. **JIT**: la JVM puede compilar a código nativo las partes más usadas.

### Ventajas y limitaciones

- Ventajas: portabilidad, verificación, optimizaciones en tiempo de ejecución.
- Limitaciones: cierto coste frente a código nativo; menos control a bajo nivel.

### Herramientas

- **javap**: desensamblador de bytecode. `javap -c MiPrograma.class`
- **ASM**: framework para manipulación y análisis de bytecode.

---

## 12. ¿Cómo funciona la _recolección de basura_ (garbage collection) en _Java_?

La **JVM** gestiona la memoria de forma automática: identifica y recupera objetos que ya no se usan.

### Conceptos clave

- **Alcance**: un objeto está "vivo" si es alcanzable desde una raíz (referencias en pilas, estáticas, etc.). Los no alcanzables son candidatos a GC.
- **Tipos de referencias**:
  - **Fuertes**: las habituales (`Object obj = new Object()`). No se recolectan.
  - **Suaves** (`SoftReference`): se recolectan si la JVM necesita memoria.
  - **Débiles** (`WeakReference`): se recolectan en el siguiente ciclo si no son alcanzables.
  - **Fantasmas** (`PhantomReference`): poco usadas; suelen usarse con `ReferenceQueue`.
- **Finalización**: el método `finalize()` está en desuso; es preferible usar `java.lang.ref.Cleaner` (Java 9+).

### Ejemplo de tipos de referencia

```java
import java.lang.ref.*;

Object obj = new Object();
SoftReference<Object> softRef = new SoftReference<>(obj);
WeakReference<Object> weakRef = new WeakReference<>(obj);
PhantomReference<Object> phantomRef = new PhantomReference<>(obj, new ReferenceQueue<>());
obj = null;  // el objeto pasa a ser candidato a GC
```

---

## 13. ¿Para qué sirve la palabra clave _final_?

**final** restringe modificaciones y aporta inmutabilidad:

- **Clase**: la clase no puede ser extendida.
- **Método**: no puede ser sobrescrito.
- **Variable**: valor constante (primitivos) o referencia constante (objetos).

### Ventajas

- Mayor seguridad y claridad de diseño.
- Posible beneficio en concurrencia y optimizaciones del JIT.

### Ejemplo

```java
class Parent {
    public final void doTask() {
        System.out.println("Método de la clase padre");
    }

    public final String name = "John";

    public final void display() {
        System.out.println("Nombre: " + name);
    }
}

class Child extends Parent {
    // public void doTask() { ... }  // error: no se puede sobrescribir un método final
    // name = "Sara";  // error: no se puede reasignar
}
```

---

## 14. ¿Se pueden _sobrecargar_ o _sobrescribir_ los métodos _estáticos_ en _Java_?

**Sobrecarga (overload): sí.** Puedes definir varios métodos estáticos con el mismo nombre y distinta firma (parámetros) en la misma clase o en una subclase.

**Sobrescritura (override): no.** Los métodos estáticos se resuelven en **tiempo de compilación** según el tipo de la referencia (enlace estático). No participan en polimorfismo: no se “sobrescriben”, sino que la subclase puede declarar un método estático con el mismo nombre que oculta el de la superclase, pero la invocación depende del tipo declarado de la variable, no del objeto real.

### Ejemplo

```java
class Parent {
    static void method() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void method() {
        System.out.println("Child");
    }
}

Parent ref = new Child();
ref.method();  // imprime "Parent", no "Child"
```

Si quitas `static` y conviertes los métodos en de instancia, entonces sí hay override y `ref.method()` llamaría al método de `Child`.

---

## 15. ¿Qué significado tiene la palabra clave _this_ en _Java_?

**this** es una referencia a la **instancia actual** de la clase. Se usa para:

### 1. Diferenciar atributos de parámetros o locales

```java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }
}
```

### 2. Llamar a otros métodos de la misma clase

```java
this.display(result);
```

### 3. Pasar la instancia actual como argumento

```java
db.update(this);
```

### 4. Encadenar constructores

```java
public Rectangle() {
    this(1, 1);
}

public Rectangle(int width, int height) {
    this.width = width;
    this.height = height;
}
```

### 5. Devolver la instancia (encadenamiento de métodos)

```java
public StringBuilder append(String s) {
    str += s;
    return this;
}
```

---

[← Volver al README principal](../../README.md)
