# JDK, JRE, JVM

El ecosistema de ejecución de Java se compone de tres piezas relacionadas: el JDK (kit de desarrollo), el JRE (entorno de ejecución) y la JVM (máquina virtual). Entender su relación ayuda a elegir qué instalar según si desarrollas o solo ejecutas aplicaciones.

## Tabla resumen

| Componente | Qué es                               | Contiene                                                             | Quién lo usa                        |
| ---------- | ------------------------------------ | -------------------------------------------------------------------- | ----------------------------------- |
| **JVM**    | Máquina virtual que ejecuta bytecode | Intérprete, JIT, GC, gestión de memoria                              | Cualquier .class/.jar               |
| **JRE**    | Entorno de ejecución                 | JVM + bibliotecas estándar (java.lang, I/O, etc.)                    | Usuario final que solo ejecuta apps |
| **JDK**    | Kit de desarrollo                    | JRE + compilador (javac), herramientas (jar, jdb, etc.), fuentes/API | Desarrollador que compila y depura  |

**Relación:** JDK ⊃ JRE ⊃ JVM. El JDK incluye el JRE; el JRE incluye la JVM.

---

## JVM (Java Virtual Machine)

Es la máquina virtual que ejecuta el bytecode compilado (.class). No “entiende” código fuente Java, solo bytecode. Proporciona portabilidad (“write once, run anywhere”): el mismo .class corre en cualquier sistema con una JVM para esa arquitectura.

**Ejemplo (vida real):** Como un traductor simultáneo: recibe instrucciones en un formato estándar (bytecode) y las ejecuta en el idioma de la máquina (código nativo).

**Características relevantes:**

- **JIT (Just-In-Time):** compila a código nativo las partes más usadas para mayor rendimiento.
- **Garbage Collector:** gestiona la memoria heap sin que el programador libere objetos manualmente.
- **Especificación:** el comportamiento de la JVM está definido por especificación; hay varias implementaciones (HotSpot, OpenJ9, etc.).

---

## JRE (Java Runtime Environment)

Incluye la JVM más las bibliotecas de clases estándar necesarias para ejecutar aplicaciones Java (por ejemplo `java.lang`, `java.util`, I/O, concurrencia). No incluye compilador ni herramientas de desarrollo.

**Ejemplo (vida real):** Como el “reproductor” de una consola: no te permite crear juegos, pero sí ejecutar los que ya están compilados.

Hasta Java 8 era habitual distribuir un JRE aparte para usuarios finales. Desde Java 9 y el módulo jlink, se suelen generar imágenes personalizadas (runtime más pequeño) en lugar de instalar un JRE completo.

---

## JDK (Java Development Kit)

Incluye todo el JRE más las herramientas para desarrollar: compilador (`javac`), empaquetado (`jar`), documentación, depurador, etc. Es lo que instala un desarrollador para escribir, compilar y depurar código Java.

**Ejemplo (código):** Flujo típico con el JDK:

```bash
# Compilar (usa javac del JDK)
javac Main.java

# Ejecutar (usa la JVM que viene en el JDK/JRE)
java Main
```

---

[← Volver al README principal](../README.md)
