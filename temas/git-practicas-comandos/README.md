# Git: prácticas, comandos y flujos de trabajo

Git es un sistema de control de versiones distribuido. Dominar los comandos principales y los flujos de trabajo permite colaborar con claridad, mantener un historial limpio y resolver conflictos de forma ordenada.

## Comandos principales

| Comando                           | Uso habitual                                                      |
| --------------------------------- | ----------------------------------------------------------------- |
| `git clone <url>`                 | Clonar un repositorio remoto                                      |
| `git branch`                      | Listar ramas; `-a` incluye remotas                                |
| `git checkout -b <rama>`          | Crear y cambiar a una rama nueva                                  |
| `git add .` / `git add <archivo>` | Añadir cambios al área de staging                                 |
| `git commit -m "mensaje"`         | Crear commit con los cambios en staging                           |
| `git push origin <rama>`          | Enviar commits al remoto                                          |
| `git pull`                        | Traer y fusionar cambios del remoto (fetch + merge)               |
| `git merge <rama>`                | Fusionar otra rama en la actual                                   |
| `git rebase <rama>`               | Reaplicar tus commits sobre otra rama (reescribe historial local) |
| `git stash`                       | Guardar cambios en pila temporal sin commitear                    |
| `git stash pop`                   | Recuperar el último stash y quitarlo de la pila                   |
| `git log`                         | Ver historial; `--oneline` resumido                               |
| `git diff`                        | Ver diferencias (working tree vs staging, o entre ramas)          |
| `git tag <nombre>`                | Crear etiqueta en el commit actual                                |

## Flujos de trabajo habituales

| Flujo           | Idea principal                                                                                                                                                                         | Uso típico                                 |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| **Git Flow**    | Rama `main` (producción estable) y rama **develop** como rama de integración: las ramas feature se fusionan en `develop`; desde ahí se crean release/hotfix y luego se lleva a `main`. | Proyectos con releases versionados         |
| **GitHub Flow** | Una rama principal (`main`); ramas cortas por feature y PR                                                                                                                             | Repos más simples, despliegue continuo     |
| **Trunk-based** | Pocas ramas, integración muy frecuente en la troncal                                                                                                                                   | Equipos que priorizan integración continua |

## Buenas prácticas

- **Commits atómicos:** Un commit = un cambio lógico; mensajes claros (imperativo, en español o inglés según convención).
- **No commitear secretos:** Usar `.gitignore` y variables de entorno; revisar antes de push.
- **Pull antes de push:** Evitar conflictos en remoto haciendo `git pull --rebase` o `git pull` antes de subir.
- **Resolución de conflictos:** Abrir los archivos marcados, elegir versiones o combinar; después `git add` y completar merge/rebase.
- **Proteger `main`:** Requerir pull requests y revisión; opcionalmente CI que deba pasar antes de merge.

---

[← Volver al README principal](../../README.md)
