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

Las rutas se configuran en el router con `canActivate: [authGuard]` o la función equivalente. Lazy loading se hace con `loadComponent` o `loadChildren` para reducir el bundle inicial.

---

[← Volver al README principal](../../README.md)
