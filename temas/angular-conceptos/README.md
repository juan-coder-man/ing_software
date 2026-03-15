# Angular: conceptos principales

Angular es un framework para aplicaciones web basado en TypeScript. Ofrece enlace de datos, rutas, HTTP, formularios reactivos y un ecosistema unificado. Las versiones recientes añaden signals, control flow nuevo y componentes standalone como estándar.

## Conceptos clave

| Concepto                  | Descripción                                                                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Signals**               | Primitiva reactiva (desde Angular 16+): `signal()`, `computed()`, `effect()`; evitan zone.js para ese estado y mejoran rendimiento. |
| **Observables**           | Flujos asíncronos; se usan con RxJS para HTTP, eventos y estado. Ver [RxJS](../rxjs/README.md).                                     |
| **Pipes**                 | Transforman datos en la plantilla (`date`, `currency`, `async`, pipes personalizados).                                              |
| **Interceptors**          | Middleware HTTP: añadir headers, token, logging o manejo global de errores.                                                         |
| **Guards**                | Protegen rutas: `CanActivate`, `CanMatch`; redirigen o deniegan según auth o roles.                                                 |
| **Rutas**                 | Configuración de rutas, lazy loading con `loadChildren`, resolvers para datos previos a la vista.                                   |
| **Standalone components** | Componentes que no dependen de `NgModule`; desde Angular 15+ recomendado para nuevo código.                                         |
| **Control flow**          | Nueva sintaxis `@if`, `@for`, `@switch` en plantillas (Angular 17+) en lugar de `*ngIf`/`*ngFor`.                                   |

## Detalle por concepto

### Signals

- Primitiva reactiva que almacena un valor y notifica a los consumidores cuando cambia.
- `computed()`: deriva un valor a partir de otras signals; se recalcula solo cuando sus dependencias cambian.
- `effect()`: ejecuta un callback cuando las signals que lee cambian (side effects: logging, sincronización).
- **Cuándo se ejecutan:** al leer una signal en plantilla o en computed/effect, Angular rastrea dependencias; con `set()` o `update()` se re-evalúan los computed y se disparan los effects.
- No dependen del ciclo de detección de cambios de zone.js para ese estado.

### Observables

- Representan flujos de valores en el tiempo.
- Uso: peticiones HTTP (`HttpClient` devuelve `Observable`), eventos del router, formularios reactivos.
- **Cuándo se ejecutan:** no hacen nada hasta que alguien se suscribe (`subscribe()` o pipe `async` en plantilla); la suscripción se activa en el momento del subscribe.
- La plantilla con `async` se actualiza cuando el Observable emite.
- Ver [RxJS](../rxjs/README.md).

### Pipes

- Transforman un valor en la plantilla para mostrarlo (fechas, moneda, mayúsculas, etc.).
- **Puros (por defecto):** Angular solo re-ejecuta el pipe cuando la referencia del valor de entrada cambia (o primitivos distintos).
- **Impuros (`pure: false`):** se re-ejecutan en cada ciclo de detección de cambios (útil pero costoso; evitar si hay muchos datos).
- **Cuándo se reevalúan:** los puros solo cuando cambia el input según igualdad referencial; los impuros en cada detección que afecte al nodo donde se usa el pipe.
- Pipe personalizado: clase con `@Pipe`, implementar `transform(value, ...args)` y declararlo (o importar en standalone).

### Interceptors

- Middleware para todas las peticiones HTTP salientes (y opcionalmente respuestas).
- **Cuándo se ejecutan:** en cada llamada a `HttpClient`; la request pasa por todos los interceptors en orden antes de salir; la response pasa por ellos en orden inverso al volver.
- Uso: añadir headers (Authorization), logging, retry o manejo global de errores.
- Registro: `provideHttpClient(withInterceptors([...]))`.

### Guards

- Protegen rutas: permiten, deniegan la navegación o redirigen.
- **Tipos y cuándo se llaman** (ver sección [Guards](#guards) más abajo):
  - `CanActivate` / `CanActivateFn`: antes de activar la ruta.
  - `CanActivateChild`: antes de activar cada ruta hija.
  - `CanDeactivate`: antes de salir de la ruta.
  - `CanMatch`: antes de que el router considere la ruta como coincidente; evita cargar el módulo si no se cumple.
- Registro en la configuración de rutas: `canActivate`, `canDeactivate`, `canMatch`, etc.

### Rutas

- Configuración del router: path, component, children.
- **Lazy loading:** `loadComponent: () => import('./ruta/foo.component').then(m => m.FooComponent)` o `loadChildren: () => import('./ruta/feature.routes').then(m => m.featureRoutes)` para cargar el código solo cuando se navega a esa ruta y reducir el bundle inicial.
- **Resolvers:** funciones que resuelven datos antes de activar la ruta; se definen con `resolve: { clave: miResolver }` en la ruta; los datos están en `route.snapshot.data['clave']` o `route.data` en el componente.
- Orden: el resolver se ejecuta después de que los guards permitan la navegación y antes de renderizar el componente; la vista tiene datos listos al mostrarse.
- Nota: el resolver clásico está deprecado; preferir resolver datos en el componente o con guards que inyecten datos.

### Standalone components

- Se declaran con `standalone: true` e importan dependencias (componentes, pipes, directivas) en el array `imports` del decorador.
- No requieren estar declarados en un `NgModule`.
- Desde Angular 15+ es el modelo recomendado para nuevo código.
- Bootstrap: `bootstrapApplication(AppComponent, appConfig)` con rutas y providers en la config.

### Control flow

- Nueva sintaxis en plantillas (Angular 17+): `@if (condición) { ... } @else { ... }`, `@for (item of items; track item.id) { ... }`, `@switch (valor) { @case (a) { ... } @default { ... } }`.
- Equivalen a `*ngIf` / `*ngFor` / `*ngSwitch` con mejor rendimiento y sintaxis más clara; permiten bloques `@else` sin wrapper adicional.
- El compilador genera código más eficiente y evita problemas típicos de `*ngIf` con múltiples elementos.

## Guards

Tipos de guards (en versiones recientes se usan funciones inyectables: `CanActivateFn`, `CanDeactivateFn`, `CanMatchFn`; las interfaces de clase siguen disponibles):

| Tipo                                          | Cuándo se llama                                                                                                                           | Uso típico                                                             |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **CanActivate** / **CanActivateFn**           | Antes de activar la ruta (navegación hacia ella). Si devuelve `false` o `UrlTree`, se cancela o se redirige.                              | Comprobar si el usuario está autenticado antes de entrar a /dashboard. |
| **CanActivateChild** / **CanActivateChildFn** | Antes de activar cada ruta hija. Se usa en rutas padre para proteger todas las hijas.                                                     | Comprobar rol antes de cargar cualquier ruta bajo /admin.              |
| **CanDeactivate** / **CanDeactivateFn**       | Antes de salir de la ruta (navegación a otra ruta). Recibe el componente actual para inspeccionar estado (ej. formulario sucio).          | Preguntar "¿Guardar cambios?" al salir de un formulario.               |
| **CanMatch** / **CanMatchFn**                 | Antes de que el router considere la ruta como coincidente. Si devuelve `false`, la ruta no se matchea y no se carga el módulo/componente. | Evitar cargar el bundle de /admin si el usuario no es admin.           |
| **Resolve** (deprecado)                       | Antes de activar la ruta, para resolver datos. Sustituir por resolver en componente o por guard que inyecte datos.                        | —                                                                      |

**Orden de ejecución:** al navegar hacia una ruta, primero se evalúa `CanMatch` (si está definido); si pasa, se cargan lazy modules y luego `CanActivate` / `CanActivateChild`. Al salir de una ruta, se evalúa `CanDeactivate`.

**Ejemplo CanActivate (función):** ya mostrado arriba con `authGuard` y `canActivate: [authGuard]`.

**Ejemplo CanDeactivate:**

```ts
export const unsavedGuard: CanDeactivateFn<FormComponent> = (component) => {
  return component.form.dirty ? confirm("¿Salir sin guardar?") : true;
};
// En la ruta: canDeactivate: [unsavedGuard]
```

**Ejemplo CanMatch:**

```ts
export const adminMatchGuard: CanMatchFn = () =>
  inject(AuthService).hasRole("admin");
// En la ruta: canMatch: [adminMatchGuard]
```

Así, si el usuario no es admin, la ruta no se matchea y no se ejecuta `loadChildren` de esa ruta.

## Ejemplo: Signal y computed

```ts
import { Component, signal, computed } from "@angular/core";

@Component({
  selector: "app-counter",
  standalone: true,
  template: `<p>Count: {{ count() }}</p>
    <p>Double: {{ double() }}</p>`,
})
export class CounterComponent {
  count = signal(0);
  double = computed(() => this.count() * 2);
}
```

## Ejemplo: Interceptor HTTP

```ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(
    req: HttpRequest<unknown>,
    next: HttpHandler,
  ): Observable<HttpEvent<unknown>> {
    const token = this.auth.getToken();
    const cloned = token
      ? req.clone({ setHeaders: { Authorization: `Bearer ${token}` } })
      : req;
    return next.handle(cloned);
  }
}
```

## Ejemplo: Guard de ruta

```ts
export const authGuard: CanActivateFn = (route, state) => {
  return inject(AuthService).isLoggedIn()
    ? true
    : inject(Router).createUrlTree(["/login"]);
};
```

Las rutas se configuran en el router con `canActivate: [authGuard]`; ver sección [Guards](#guards) para el resto de tipos. Lazy loading: `loadComponent` o `loadChildren` en la definición de la ruta.

---

[← Volver al README principal](../../README.md)
