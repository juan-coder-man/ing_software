# Complejidad ciclomática y complejidad anidada

La **complejidad ciclomática** mide la cantidad de caminos independientes en el código; la **complejidad anidada** (anidación) afecta la legibilidad y el mantenimiento. Ambas se usan para detectar funciones o métodos que conviene dividir o simplificar.

## Complejidad ciclomática

- **Definición:** Número de caminos linealmente independientes que puede seguir el flujo (grafo de control). Fórmula habitual: `CC = 1 + número de puntos de decisión` (if, else, switch case, while, for, `&&`, `||`, `?:`, etc.).
- **Uso:** Umbrales típicos por función: 1–10 aceptable, 11–20 revisar, >20 alto riesgo. Herramientas como SonarQube o linters la calculan.
- **Objetivo:** Reducir ramas y decisiones; extraer métodos, usar early return o reemplazar condicionales por polimorfismo/estrategias cuando aplique.

### Ejemplo real: validación de formulario (CC alta)

Función que valida varios campos en un solo método: cada condición suma a la CC. Con 5 if/else la CC ≈ 6; añadiendo más reglas se dispara.

**Antes (CC alta, difícil de testear):**

```java
public List<String> validarRegistro(String email, Integer edad, String pais, Boolean aceptaTerminos) {
    List<String> errores = new ArrayList<>();
    if (email == null || email.isBlank()) {
        errores.add("Email requerido");
    } else if (!email.contains("@") || !email.contains(".")) {
        errores.add("Email inválido");
    }
    if (edad == null) {
        errores.add("Edad requerida");
    } else if (edad < 18 || edad > 120) {
        errores.add("Edad debe estar entre 18 y 120");
    }
    if (pais == null || pais.isBlank()) {
        errores.add("País requerido");
    }
    if (aceptaTerminos == null || !aceptaTerminos) {
        errores.add("Debe aceptar los términos");
    }
    return errores;
}
```

**Después (CC baja por función, más legible y testeable):**

```java
public List<String> validarRegistro(String email, Integer edad, String pais, Boolean aceptaTerminos) {
    List<String> errores = new ArrayList<>();
    validarEmail(email, errores);
    validarEdad(edad, errores);
    if (pais == null || pais.isBlank()) errores.add("País requerido");
    if (aceptaTerminos == null || !aceptaTerminos) errores.add("Debe aceptar los términos");
    return errores;
}

private void validarEmail(String email, List<String> errores) {
    if (email == null || email.isBlank()) {
        errores.add("Email requerido");
        return;
    }
    if (!email.contains("@") || !email.contains(".")) errores.add("Email inválido");
}

private void validarEdad(Integer edad, List<String> errores) {
    if (edad == null) {
        errores.add("Edad requerida");
        return;
    }
    if (edad < 18 || edad > 120) errores.add("Edad debe estar entre 18 y 120");
}
```

Cada método extraído tiene menos puntos de decisión; la CC global se reparte y los tests pueden probar cada validación por separado.

### Ejemplo real: cálculo de descuento (muchas condiciones)

Método que decide el porcentaje de descuento según tipo de cliente, antigüedad y monto: muchas ramas implican CC alta.

**Antes:** Varios `if`/`else if` encadenados (CC > 10). **Después:** Tabla de decisiones (mapa o lista de reglas) o estrategias por tipo de cliente; un único `if` por regla o un lookup por clave, reduciendo la CC.

```java
// Refactor: tabla de reglas (ejemplo simplificado)
public int descuentoPorcentaje(String tipoCliente, int añosAntiguedad, BigDecimal monto) {
    if ("VIP".equals(tipoCliente) && añosAntiguedad >= 5 && monto.compareTo(new BigDecimal("1000")) >= 0)
        return 20;
    if ("VIP".equals(tipoCliente) && añosAntiguedad >= 2) return 15;
    if ("Premium".equals(tipoCliente) && monto.compareTo(new BigDecimal("500")) >= 0) return 10;
    if ("Premium".equals(tipoCliente)) return 5;
    return 0;
}
```

Se puede bajar aún más la CC extrayendo cada regla a un método `aplicaDescuentoVIP`, `aplicaDescuentoPremium`, etc., o usando una lista de reglas evaluadas en orden.

## Complejidad anidada (anidación)

- **Definición:** Nivel de anidación de bloques (if dentro de if, loops dentro de if, etc.). Muchas herramientas cuentan la profundidad máxima (p. ej. "nesting depth").
- **Problema:** Anidación alta (p. ej. > 3–4 niveles) dificulta la lectura y las pruebas; suele indicar que la función hace demasiado.
- **Cómo reducir:** Early return para casos de error o borde; extraer lógica a funciones con nombre claro; evitar anidar más de lo necesario.

### Ejemplo real: procesamiento de pedido / respuesta HTTP

Se recibe un pedido o una respuesta; hay que comprobar null, estado, permisos y luego ejecutar la lógica. Anidando todo se llega a 4 niveles.

**Antes (anidación alta):**

```java
public void procesarPedido(Pedido pedido, Usuario usuario) {
    if (pedido != null) {
        if (pedido.getEstado() == EstadoPedido.PENDIENTE) {
            if (usuario.tienePermiso("pedido.procesar")) {
                aplicarDescuentos(pedido);
                actualizarStock(pedido.getLineas());
                pedido.setEstado(EstadoPedido.PROCESADO);
            }
        }
    }
}
```

**Después (early return + mismo nivel):**

```java
public void procesarPedido(Pedido pedido, Usuario usuario) {
    if (pedido == null) return;
    if (pedido.getEstado() != EstadoPedido.PENDIENTE) return;
    if (!usuario.tienePermiso("pedido.procesar")) return;

    aplicarDescuentos(pedido);
    actualizarStock(pedido.getLineas());
    pedido.setEstado(EstadoPedido.PROCESADO);
}
```

La lógica “feliz” queda al mismo nivel; los guardas salen pronto y se leen como precondiciones.

### Ejemplo real: bucle con condicionales anidados

Se recorren ítems y solo para algunos se comprueba stock y permisos antes de actualizar. Varios niveles dentro del `for` dificultan la lectura.

**Antes (anidación dentro del bucle):**

```java
for (LineaPedido linea : pedido.getLineas()) {
    if (linea.getCantidad() > 0) {
        Producto p = productoRepository.findById(linea.getProductoId());
        if (p != null && p.getStock() >= linea.getCantidad()) {
            if (usuario.puedeComprar(p)) {
                actualizarLinea(linea, p);
            }
        }
    }
}
```

**Después (extraer “¿puedo procesar?” + early continue):**

```java
for (LineaPedido linea : pedido.getLineas()) {
    if (linea.getCantidad() <= 0) continue;
    if (!puedeProcesarLinea(linea, usuario)) continue;

    Producto p = productoRepository.findById(linea.getProductoId());
    actualizarLinea(linea, p);
}

private boolean puedeProcesarLinea(LineaPedido linea, Usuario usuario) {
    Producto p = productoRepository.findById(linea.getProductoId());
    return p != null && p.getStock() >= linea.getCantidad() && usuario.puedeComprar(p);
}
```

El bucle expresa “para cada línea válida y procesable, actualizar”; los detalles de stock y permisos quedan en un método con nombre claro.

### Ejemplo mínimo (evitar)

```java
if (a != null) {
    if (b != null) {
        if (c.isValid()) {
            // lógica muy dentro
        }
    }
}
```

**Mejor:** `if (a == null || b == null || !c.isValid()) return;` y luego la lógica principal al mismo nivel.

---

[← Volver al README principal](../../README.md)
