# Lenguaje Go

Go (Golang) es un lenguaje compilado, estático y de sintaxis sencilla creado en Google. Destaca por la concurrencia nativa (goroutines, channels), compilación rápida, un solo ejecutable y una biblioteca estándar amplia. Se usa en servicios, herramientas CLI, contenedores (Docker, Kubernetes) y APIs.

## Características principales

| Aspecto             | Descripción                                                                   |
| ------------------- | ----------------------------------------------------------------------------- |
| **Tipado estático** | Tipos en tiempo de compilación; inferencia con `:=`.                          |
| **Concurrencia**    | Goroutines (ligeras) y channels para comunicación; no hay locks obligatorios. |
| **Módulos**         | Desde Go 1.11+, `go mod` para dependencias y versionado.                      |
| **Un solo binario** | Compilación estática por defecto; despliegue sencillo.                        |

## Sintaxis básica

```go
package main

import "fmt"

func main() {
    x := 42
    fmt.Println("Hola:", x)
}

func add(a, b int) int {
    return a + b
}
```

## Concurrencia: goroutines y channels

```go
ch := make(chan int)
go func() { ch <- 1 }()
result := <-ch
```

- **Goroutine:** `go f()` lanza `f` en otra goroutine.
- **Channel:** `make(chan T)`; enviar `ch <- v`, recibir `v := <-ch`. Pueden ser buffered: `make(chan int, 10)`.

## Módulos

```bash
go mod init ejemplo.com/mi-app
go get github.com/alguna/lib@v1.2.3
```

`go.mod` y `go.sum` gestionan dependencias; el código importa con la ruta del módulo.

## Casos de uso típicos

- Microservicios y APIs (HTTP con `net/http` o frameworks como Gin, Echo).
- Herramientas de línea de comandos.
- Procesamiento concurrente (workers, pipelines).
- Infraestructura: Docker, Kubernetes, Terraform y muchas herramientas DevOps están escritas en Go.

---

[← Volver al README principal](../../README.md)
