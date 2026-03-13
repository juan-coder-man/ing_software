# Diferencias entre Hibernate y JPA

**JPA** (Java Persistence API) es una **especificación** que define cómo mapear objetos Java a tablas y cómo interactuar con una base de datos relacional. **Hibernate** es una **implementación** de esa especificación: el código que realmente ejecuta las operaciones. Usar JPA en el código permite cambiar de implementación (por ejemplo a EclipseLink) sin reescribir la lógica de persistencia.

## Tabla comparativa

| Aspecto          | JPA                                                                         | Hibernate                                                     |
| ---------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Naturaleza       | Especificación (API / contrato)                                             | Implementación (ORM concreto)                                 |
| Quién lo define  | Jakarta EE (antes Java EE)                                                  | Red Hat / comunidad                                           |
| Anotaciones      | `@Entity`, `@Table`, `@Id`, `@OneToMany`, etc.                              | Las de JPA + extensiones propias (`@Formula`, `@Where`, etc.) |
| API de consultas | JPQL, Criteria API                                                          | JPQL, Criteria, HQL, API nativa                               |
| Uso en código    | Se programa contra interfaces JPA (`EntityManager`, `EntityManagerFactory`) | Se puede usar solo JPA o APIs propias de Hibernate            |
| Portabilidad     | Código portable entre implementaciones JPA                                  | Funciones propias de Hibernate no son estándar                |

---

## JPA (Java Persistence API)

Es el estándar: define anotaciones, el ciclo de vida de las entidades, JPQL, transacciones y el contrato del `EntityManager`. El código que solo usa JPA puede ejecutarse sobre Hibernate, EclipseLink, OpenJPA u otra implementación compatible.

**Ejemplo (vida real):** JPA es como el estándar USB: defines conectores y protocolo; Hibernate es un dispositivo concreto que cumple ese estándar.

**Ejemplo (código):** Entidad pura JPA:

```java
@Entity
@Table(name = "usuarios")
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany(mappedBy = "usuario", cascade = CascadeType.ALL)
    private List<Pedido> pedidos = new ArrayList<>();
    // getters, setters
}
```

---

## Hibernate

Hibernate es el ORM que implementa JPA y añade características propias: HQL con extensiones, filtros (`@Where`), fórmulas (`@Formula`), tipos de usuario, integración con validación (Bean Validation), etc. En la práctica, muchas aplicaciones Spring Boot usan “JPA” con Hibernate como proveedor por defecto.

**Relación:** Cuando usas Spring Data JPA con el starter por defecto, estás usando JPA como API y Hibernate como implementación. Puedes limitarte a JPA para mayor portabilidad o usar extensiones de Hibernate cuando necesites funcionalidad no estándar.

---

[← Volver al README principal](../README.md)
