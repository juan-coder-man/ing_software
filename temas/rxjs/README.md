# RxJS

RxJS es una librería para programación reactiva con **Observables**: flujos de eventos o valores en el tiempo que se pueden componer, transformar y cancelar. En Angular se usa para HTTP, formularios reactivos, eventos y estado asíncrono.

## Observable vs Promise

| Aspecto     | Promise              | Observable                                 |
| ----------- | -------------------- | ------------------------------------------ |
| Valores     | Uno (resolve/reject) | Múltiples a lo largo del tiempo            |
| Ejecución   | Inmediata al crear   | Al suscribirse (lazy)                      |
| Cancelación | No estándar          | `unsubscribe()` o operadores que completan |
| Composición | Encadenar `.then`    | Operadores (map, filter, switchMap, etc.)  |

## Creación y suscripción

```ts
import { of, from, interval } from "rxjs";

of(1, 2, 3).subscribe((v) => console.log(v));
from([1, 2, 3]).subscribe((v) => console.log(v));
const sub = interval(1000).subscribe((n) => console.log(n));
// Cancelar: sub.unsubscribe();
```

## Operadores habituales

| Operador                 | Uso                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------- |
| `map`                    | Transformar cada valor.                                                               |
| `filter`                 | Filtrar valores.                                                                      |
| `switchMap`              | Cambiar a otro Observable (p. ej. petición HTTP por cada valor); cancela la anterior. |
| `mergeMap` / `concatMap` | Aplanar Observables; merge en paralelo, concat en orden.                              |
| `combineLatest`          | Combinar los últimos valores de varios Observables.                                   |
| `takeUntil`              | Completar cuando otro Observable emite (útil para cancelar al destruir componente).   |

## Suscripciones y memoria

Suscribirse sin desuscribir en componentes puede causar fugas de memoria. Buenas prácticas:

- Usar `async` pipe en la plantilla (Angular se desuscribe al destruir).
- Usar `takeUntil(destroy$)` con un `Subject` que se complete en `ngOnDestroy`.
- En Angular 16+, `DestroyRef` + `takeUntilDestroyed()` para componentes standalone o inyectables.

## Subjects

Un **Subject** es Observable y Observer: emite valores con `.next()` y otros se suscriben. Variantes:

- **Subject:** sin valor inicial; emite solo a suscriptores actuales.
- **BehaviorSubject:** tiene valor actual; nuevos suscriptores lo reciben al instante.
- **ReplaySubject:** repite las últimas N emisiones a nuevos suscriptores.

---

[← Volver al README principal](../../README.md)
