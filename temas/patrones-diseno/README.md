# Patrones de Diseño

Los patrones de diseño son soluciones recurrentes a problemas comunes de diseño de software. Documentan buenas prácticas y facilitan la comunicación entre desarrolladores. Suelen clasificarse en creacionales, estructurales y de comportamiento.

## Resumen de patrones

| Patrón           | Categoría      | En una frase                                                                  | Ejemplo vida real                                                                                            | Descripción ejemplo técnico                                                                                      |
| ---------------- | -------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Singleton        | Creacional     | Una única instancia global de una clase.                                      | Un único panel de control de una máquina: todos acceden al mismo estado.                                     | Clase con instancia estática y constructor privado; método estático `obtener()` devuelve esa instancia.          |
| Factory Method   | Creacional     | La subclase decide qué clase concreta instanciar.                             | Fábrica de vehículos: el cliente pide "un vehículo" y recibe coche, moto o camión según criterio interno.    | `Creador` con `factoryMethod()` que las subclases implementan para devolver el `Producto` concreto.              |
| Abstract Factory | Creacional     | Familias de objetos relacionados sin acoplar a implementaciones concretas.    | Kit de UI (tema claro/oscuro): la fábrica devuelve botones y campos coherentes entre sí.                     | `GUIFactory` con `crearBoton()` y `crearCampo()`; `FactoryTemaClaro` devuelve componentes del tema claro.        |
| Builder          | Creacional     | Construcción paso a paso de objetos complejos con muchos parámetros.          | Montar un menú: añadir entrante, principal, postre y bebida en el orden que se quiera.                       | `MensajeBuilder` con setters encadenables (`setDestinatario`, `setAsunto`) y `build()` que devuelve el objeto.   |
| Prototype        | Creacional     | Crear nuevos objetos clonando una instancia existente.                        | Plantilla de documento: duplicar la plantilla y editar la copia en lugar de configurar todo desde cero.      | Interfaz `Prototype` con `clonar()`; `Documento` devuelve una nueva instancia con el mismo contenido.            |
| Adapter          | Estructural    | Adaptar la interfaz de una clase a la que el cliente espera.                  | Adaptador de enchufe: el dispositivo tiene una clavija; el adaptador expone la interfaz que espera la pared. | `Adapter` implementa `Objetivo` y delega en `Adaptado.peticionEspecifica()` para cumplir `peticion()`.           |
| Decorator        | Estructural    | Añadir responsabilidades a un objeto de forma dinámica, envolviéndolo.        | Café con leche, con nata, con extra: cada adorno envuelve el anterior sin cambiar la clase base.             | `DecoratorA` implementa `Componente`, envuelve otro `Componente` y amplía `operacion()` llamando al wrappee.     |
| Facade           | Estructural    | Interfaz simplificada para un subsistema complejo.                            | Un solo botón "reproducir" que enciende equipo, selecciona fuente y arranca el contenido.                    | `Facade` con `operacionCompleja()` que invoca en secuencia a `SubsistemaA` y `SubsistemaB`.                      |
| Proxy            | Estructural    | Objeto sustituto que controla el acceso al objeto real.                       | Representante de un artista: las peticiones pasan por él; filtra, registra o delega.                         | `Proxy` implementa `Sujeto` y delega en `RealSujeto`; puede añadir lazy init, caché o control de acceso.         |
| Composite        | Estructural    | Componer objetos en estructuras de árbol (parte-todo).                        | Carpeta con archivos y subcarpetas: "mostrar tamaño" se aplica igual a archivo o carpeta (sumando hijos).    | `Composite` mantiene lista de `Componente`; `ejecutar()` recorre los hijos; `Hoja` y `Composite` misma interfaz. |
| Bridge           | Estructural    | Separar abstracción de implementación para que varíen de forma independiente. | Remoto (abstracción) y dispositivo (TV, radio): cada uno puede cambiar sin multiplicar clases.               | `Abstraccion` recibe un `Implementador`; `ejecutar()` delega en `impl.operacion()`; variantes por separado.      |
| Observer         | Comportamiento | Notificar a varios dependientes cuando el sujeto cambia de estado.            | Suscripción a newsletter: al publicar un artículo, todos los suscritos reciben la notificación.              | `Sujeto` mantiene lista de `Observador`; `notificar(datos)` llama a `actualizar(datos)` en cada uno.             |
| Strategy         | Comportamiento | Encapsular algoritmos intercambiables en tiempo de ejecución.                 | Rutas en mapa: a pie, en coche o transporte; el mismo "ir de A a B" con distintas estrategias.               | `Contexto` tiene una `Estrategia` inyectada; `operar(a, b)` delega en `estrategia.ejecutar(a, b)`.               |
| Command          | Comportamiento | Encapsular una petición como objeto para parametrizar, encolar o deshacer.    | Pedido en restaurante: la comanda pasa de camarero a cocina; se puede encolar, cancelar o repetir.           | Interfaz `Comando` con `ejecutar()`; `Invocador` recibe un comando y lo ejecuta; el receptor hace la acción.     |
| Template Method  | Comportamiento | Esqueleto de algoritmo en la base; subclases definen pasos concretos.         | Receta de pastel: pasos (mezclar, hornear, decorar) fijos; la receta concreta define qué mezclar u hornear.  | `ClaseAbstracta` con `templateMethod()` que llama a `pasoObligatorio()` y al abstracto `pasoVariable()`.         |
| State            | Comportamiento | El comportamiento del objeto cambia según su estado interno.                  | Semáforo: el mismo "siguiente" pasa a verde, ámbar o rojo según el estado actual.                            | `Contexto` tiene un `Estado`; `solicitar()` delega en `estado.manejar(contexto)`; el estado puede cambiarse.     |
| Iterator         | Comportamiento | Recorrer una colección sin exponer su representación interna.                 | Canales de TV: se recorre uno a uno sin conocer cómo está almacenada la lista internamente.                  | `Iterador` con `siguiente()` y `tieneSiguiente()`; recorre la colección manteniendo un índice interno.           |

## Clasificación por categoría

| Categoría          | Propósito                                     | Patrones                                                        |
| ------------------ | --------------------------------------------- | --------------------------------------------------------------- |
| **Creacionales**   | Creación de objetos desacoplada y flexible    | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| **Estructurales**  | Composición de clases y objetos               | Adapter, Decorator, Facade, Proxy, Composite, Bridge            |
| **Comportamiento** | Interacción y responsabilidades entre objetos | Observer, Strategy, Command, Template Method, State, Iterator   |

---

## Creacionales

### Singleton

Asegura que una clase tenga una única instancia y ofrece un punto de acceso global. Útil para recursos compartidos (conexión a BD, logger, configuración). En entornos multihilo hay que cuidar la inicialización.

**Ejemplo (vida real):** Un único panel de control de una máquina: todos acceden al mismo estado.

**Ejemplo (código):**

```ts
class Configuracion {
  private static instancia: Configuracion | null = null;

  private constructor() {}

  static obtener(): Configuracion {
    if (!Configuracion.instancia) {
      Configuracion.instancia = new Configuracion();
    }
    return Configuracion.instancia;
  }
}
```

---

## Factory Method

Delega la creación de objetos en un método (o clase factory) en lugar de usar `new` directamente. Permite que las subclases decidan qué clase concreta instanciar, desacoplando el código del tipo concreto.

**Ejemplo (vida real):** Una fábrica de vehículos: el cliente pide "un vehículo"; la fábrica devuelve coche, moto o camión según criterio interno.

**Ejemplo (código):**

```ts
interface Producto {
  operacion(): string;
}

class Creador {
  crearProducto(): Producto {
    return this.factoryMethod();
  }
  protected factoryMethod(): Producto {
    throw new Error("Subclase debe implementar factoryMethod");
  }
}

class CreadorConcreto extends Creador {
  protected factoryMethod(): Producto {
    return new ProductoConcreto();
  }
}
```

### Abstract Factory

Proporciona una interfaz para crear familias de objetos relacionados sin especificar clases concretas. Útil cuando el sistema debe ser independiente de cómo se crean, componen y representan sus productos.

**Ejemplo (vida real):** Kit de UI (tema claro/oscuro): la fábrica devuelve botones, campos y ventanas coherentes entre sí.

**Ejemplo (código):**

```ts
interface GUIFactory {
  crearBoton(): Boton;
  crearCampo(): Campo;
}
class FactoryTemaClaro implements GUIFactory {
  crearBoton() {
    return new BotonClaro();
  }
  crearCampo() {
    return new CampoClaro();
  }
}
```

### Builder

Separa la construcción de un objeto complejo de su representación, de modo que el mismo proceso de construcción pueda crear distintas representaciones. Evita constructores con muchos parámetros opcionales.

**Ejemplo (vida real):** Montar un menú: añadir entrante, plato principal, postre y bebida en el orden que se quiera, sin un constructor gigante.

**Ejemplo (código):**

```ts
class MensajeBuilder {
  private destinatario = "";
  private asunto = "";
  setDestinatario(d: string) {
    this.destinatario = d;
    return this;
  }
  setAsunto(a: string) {
    this.asunto = a;
    return this;
  }
  build() {
    return new Mensaje(this.destinatario, this.asunto);
  }
}
// Uso: new MensajeBuilder().setDestinatario("a@b.com").setAsunto("Hola").build();
```

### Prototype

Crea nuevos objetos copiando un prototipo (instancia existente). Útil cuando la creación directa es costosa o cuando se quiere independizar el código de las clases concretas.

**Ejemplo (vida real):** Plantilla de documento: duplicar la plantilla y editar la copia en lugar de configurar todo desde cero.

**Ejemplo (código):**

```ts
interface Prototype {
  clonar(): Prototype;
}
class Documento implements Prototype {
  constructor(private contenido: string) {}
  clonar(): Documento {
    return new Documento(this.contenido);
  }
}
```

---

## Estructurales

### Adapter

Adapta la interfaz de una clase existente a otra que el cliente espera. Permite que clases con interfaces incompatibles trabajen juntas sin modificar su código fuente.

**Ejemplo (vida real):** Adaptador de enchufe: el dispositivo tiene una clavija; el adaptador expone la interfaz que espera el enchufe de la pared.

**Ejemplo (código):**

```ts
interface Objetivo {
  peticion(): string;
}
class Adaptado {
  peticionEspecifica(): string {
    return "datos";
  }
}
class Adapter implements Objetivo {
  constructor(private adaptado: Adaptado) {}
  peticion(): string {
    return this.adaptado.peticionEspecifica();
  }
}
```

### Decorator

Añade responsabilidades a un objeto de forma dinámica, envolviéndolo en objetos “decoradores” que implementan la misma interfaz. Alternativa flexible a la herencia para extender comportamiento.

**Ejemplo (vida real):** Café con leche, con nata, con extra de shot: cada adorno “envuelve” el anterior sin cambiar la clase base.

**Ejemplo (código):**

```ts
interface Componente {
  operacion(): string;
}
class ComponenteConcreto implements Componente {
  operacion() {
    return "Base";
  }
}
class DecoratorA implements Componente {
  constructor(private wrappee: Componente) {}
  operacion() {
    return `DecoratorA(${this.wrappee.operacion()})`;
  }
}
```

### Facade

Proporciona una interfaz unificada y simplificada a un conjunto de interfaces de un subsistema. Reduce la complejidad que el cliente debe conocer.

**Ejemplo (vida real):** Un solo botón “reproducir” que internamente enciende el equipo, selecciona la fuente y arranca el contenido.

**Ejemplo (código):**

```ts
class SubsistemaA {
  ejecutar() {}
}
class SubsistemaB {
  ejecutar() {}
}
class Facade {
  constructor(
    private a: SubsistemaA,
    private b: SubsistemaB,
  ) {}
  operacionCompleja() {
    this.a.ejecutar();
    this.b.ejecutar();
  }
}
```

### Proxy

Proporciona un sustituto o marcador de posición para otro objeto y controla el acceso a él. Útil para lazy loading, control de acceso, logging o caché.

**Ejemplo (vida real):** Representante de un artista: las peticiones pasan por el representante, que filtra, registra o delega.

**Ejemplo (código):**

```ts
interface Sujeto {
  peticion(): void;
}
class RealSujeto implements Sujeto {
  peticion() {
    /* trabajo costoso */
  }
}
class Proxy implements Sujeto {
  constructor(private real: RealSujeto) {}
  peticion() {
    // Lazy init, caché o control de acceso
    this.real.peticion();
  }
}
```

### Composite

Compone objetos en estructuras de árbol para representar jerarquías parte-todo. El cliente trata objetos individuales y composiciones de forma uniforme.

**Ejemplo (vida real):** Carpeta que contiene archivos y otras carpetas: “mostrar tamaño” se aplica igual a un archivo o a una carpeta (sumando el de sus hijos).

**Ejemplo (código):**

```ts
interface Componente {
  ejecutar(): void;
}
class Hoja implements Componente {
  ejecutar() {
    /* acción en hoja */
  }
}
class Composite implements Componente {
  private hijos: Componente[] = [];
  agregar(c: Componente) {
    this.hijos.push(c);
  }
  ejecutar() {
    this.hijos.forEach((h) => h.ejecutar());
  }
}
```

### Bridge

Desacopla una abstracción de su implementación para que ambas puedan variar de forma independiente. Evita una explosión de subclases cuando hay varias dimensiones de variación.

**Ejemplo (vida real):** Remoto (abstracción) y dispositivo (TV, radio): el remoto puede cambiar y los dispositivos también, sin multiplicar clases “RemotoTV”, “RemotoRadio”, etc.

**Ejemplo (código):**

```ts
interface Implementador {
  operacion(): string;
}
class Abstraccion {
  constructor(protected impl: Implementador) {}
  ejecutar() {
    return this.impl.operacion();
  }
}
class ImplementacionA implements Implementador {
  operacion() {
    return "A";
  }
}
```

---

## Comportamiento

### Observer

Define una dependencia uno-a-muchos: cuando un objeto (sujeto) cambia de estado, notifica a sus observadores para que se actualicen. Útil en UIs (modelo → vistas), eventos y sistemas desacoplados.

**Ejemplo (vida real):** Suscripción a una newsletter: al publicar un nuevo artículo, todos los suscritos reciben la notificación.

**Ejemplo (código):**

```ts
interface Observador {
  actualizar(datos: unknown): void;
}

class Sujeto {
  private observadores: Observador[] = [];

  suscribir(o: Observador) {
    this.observadores.push(o);
  }

  notificar(datos: unknown) {
    this.observadores.forEach((o) => o.actualizar(datos));
  }
}
```

### Strategy

Define una familia de algoritmos, los encapsula y los hace intercambiables. La estrategia puede variar independientemente del cliente que la usa. Útil para evitar condicionales múltiples según el tipo de comportamiento.

**Ejemplo (vida real):** Rutas en un mapa: a pie, en coche o en transporte público; el mismo “ir de A a B” con distintas estrategias de cálculo.

**Ejemplo (código):**

```ts
interface Estrategia {
  ejecutar(a: number, b: number): number;
}
class Suma implements Estrategia {
  ejecutar(a: number, b: number) {
    return a + b;
  }
}
class Contexto {
  constructor(private estrategia: Estrategia) {}
  setEstrategia(e: Estrategia) {
    this.estrategia = e;
  }
  operar(a: number, b: number) {
    return this.estrategia.ejecutar(a, b);
  }
}
```

### Command

Encapsula una petición como objeto, permitiendo parametrizar clientes con distintas peticiones, encolar operaciones, implementar deshacer o registrar logs. Separa quien invoca de quien ejecuta.

**Ejemplo (vida real):** Pedido en un restaurante: la comanda (objeto) pasa de camarero a cocina; se puede encolar, cancelar o repetir sin que el camarero conozca el detalle de la cocina.

**Ejemplo (código):**

```ts
interface Comando {
  ejecutar(): void;
}
class ComandoConcreto implements Comando {
  constructor(private receptor: { accion(): void }) {}
  ejecutar() {
    this.receptor.accion();
  }
}
class Invocador {
  constructor(private comando: Comando) {}
  ejecutar() {
    this.comando.ejecutar();
  }
}
```

### Template Method

Define el esqueleto de un algoritmo en una clase base, dejando que las subclases implementen o sobrescriban pasos concretos. Evita duplicar la estructura del algoritmo en varias clases.

**Ejemplo (vida real):** Receta de pastel: los pasos (mezclar, hornear, decorar) son fijos; la receta concreta define “qué mezclar” o “cuánto hornear”.

**Ejemplo (código):**

```ts
abstract class ClaseAbstracta {
  templateMethod() {
    this.pasoObligatorio();
    this.pasoVariable();
  }
  private pasoObligatorio() {
    /* común */
  }
  protected abstract pasoVariable(): void;
}
class Concreta extends ClaseAbstracta {
  protected pasoVariable() {
    /* implementación */
  }
}
```

### State

Permite que un objeto altere su comportamiento cuando su estado interno cambia. El objeto parecerá cambiar de clase. Útil cuando el comportamiento depende de muchos estados y condicionales complejos.

**Ejemplo (vida real):** Semáforo: el mismo “siguiente” hace pasar a verde, ámbar o rojo según el estado actual.

**Ejemplo (código):**

```ts
interface Estado {
  manejar(contexto: Contexto): void;
}
class Contexto {
  constructor(private estado: Estado) {}
  setEstado(e: Estado) {
    this.estado = e;
  }
  solicitar() {
    this.estado.manejar(this);
  }
}
```

### Iterator

Proporciona una forma de acceder secuencialmente a los elementos de una colección sin exponer su representación interna. Centraliza la lógica de recorrido y permite varios iteradores sobre la misma colección.

**Ejemplo (vida real):** Canales de TV: se recorre uno a uno sin tener que conocer cómo está almacenada la lista internamente.

**Ejemplo (código):**

```ts
interface Iterador<T> {
  siguiente(): T | null;
  tieneSiguiente(): boolean;
}
class IteradorConcreto<T> implements Iterador<T> {
  constructor(
    private coleccion: T[],
    private indice = 0,
  ) {}
  tieneSiguiente() {
    return this.indice < this.coleccion.length;
  }
  siguiente() {
    return this.coleccion[this.indice++] ?? null;
  }
}
```

---

[← Volver al README principal](../../README.md)
