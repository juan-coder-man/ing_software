# Calidad de código en local

Mantener calidad de código en el entorno local se consigue con linters, análisis estático, formateadores y hooks que ejecuten estas herramientas antes de cada commit o push. Así se detectan problemas temprano y se mantiene un estilo consistente.

## Herramientas habituales

| Herramienta               | Propósito                                                                         | Ejemplo de uso                                                                       |
| ------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **SonarQube / SonarLint** | Análisis estático: bugs, vulnerabilidades, code smells, duplicación, cobertura.   | SonarLint en el IDE; SonarQube en servidor para historial y umbrales.                |
| **Linters**               | Reglas de estilo y posibles errores (variables no usadas, imports, convenciones). | ESLint (JS/TS), Pylint (Python), dart analyze (Flutter), Checkstyle/SpotBugs (Java). |
| **Formateadores**         | Formato consistente (indentación, comillas, saltos de línea).                     | Prettier (JS/TS/HTML/CSS), dart format, Black (Python), google-java-format.          |
| **Pre-commit hooks**      | Ejecutar linter, formateador o tests antes de permitir el commit.                 | Husky + lint-staged (npm), pre-commit (Python), git hooks manuales.                  |

## Flujo típico en local

1. **Al guardar o al commit:** El formateador se aplica automáticamente (guardar) o en pre-commit; el linter se ejecuta en pre-commit o en el IDE.
2. **Antes de push:** Pipeline local o CI que ejecute tests y análisis (SonarQube, linter) y falle si no se cumplen umbrales.
3. **En el IDE:** SonarLint y el linter del lenguaje muestran problemas en tiempo real; corregir antes de commitear.

## Configuración compartida

- Reglas de ESLint, Prettier, etc. en el repositorio (archivos de config) para que todo el equipo use las mismas.
- Umbrales de SonarQube (cobertura, duplicación, bugs) definidos en el proyecto y aplicados en CI; en local SonarLint usa las mismas reglas cuando está conectado al servidor.

Invertir en estas herramientas en local reduce deuda técnica y evita que lleguen a main código que luego falle en CI o en revisión.

---

[← Volver al README principal](../../README.md)
