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

## Configuración de cada herramienta

### SonarQube / SonarLint

- **SonarLint (IDE):** Instalar la extensión "SonarLint" en VS Code o Cursor (Extensions → buscar SonarLint → Install). Opcional: enlazar a un proyecto SonarQube (SonarLint: Connect to SonarQube) para heredar reglas y calidad del servidor. Los problemas se ven en el panel Problems y como subrayados en el editor.
- **SonarQube (servidor):** En la raíz del proyecto crear `sonar-project.properties`:

```properties
sonar.projectKey=mi-proyecto
sonar.sources=src
sonar.exclusions=**/node_modules/**,**/dist/**,**/*.spec.ts
sonar.host.url=https://sonar.example.com
sonar.login=<token>
```

Ejecutar el análisis: `sonar-scanner` (tras instalar el CLI) o con Maven `mvn sonar:sonar` / Gradle con plugin `sonarqube`.

### Linters

- **ESLint (JS/TS):** `npm init -y` (si no hay package.json), luego `npm install -D eslint`. Crear config: `npx eslint --init` (asistente) o crear `eslint.config.js` (flat config, ESLint 9+) o `.eslintrc.cjs` con reglas. Ejemplo mínimo en `package.json`: `"lint": "eslint ."`. Ejecutar: `npm run lint`.
- **Pylint (Python):** `pip install pylint`. Configuración en `.pylintrc` o en `pyproject.toml` bajo `[tool.pylint.messages_control]` (disable, enable). Comando: `pylint src/` o `pylint **/*.py`.
- **dart analyze (Flutter/Dart):** En la raíz del proyecto crear o editar `analysis_options.yaml`:

```yaml
include: package:flutter_lints/flutter.yaml
linter:
  rules:
    - avoid_print
    - prefer_const_constructors
```

Comando: `dart analyze`.

- **Checkstyle / SpotBugs (Java):** Con Maven: añadir plugin en `pom.xml` (checkstyle-maven-plugin, spotbugs-maven-plugin); Checkstyle usa un archivo XML de reglas (p. ej. Google Checkstyle). Comandos: `mvn checkstyle:check`, `mvn spotbugs:check`. Con Gradle: plugins `checkstyle`, `com.github.spotbugs`; configurar en `config/checkstyle/checkstyle.xml` o en el bloque del plugin.

### Formateadores

- **Prettier:** `npm install -D prettier`. Crear `.prettierrc` (o añadir `"prettier": { "semi": true, "singleQuote": true }` en `package.json`) y `.prettierignore` (ej. `dist`, `node_modules`). Script: `"format": "prettier --write ."`. Ejecutar: `npm run format`.
- **dart format:** No requiere config en proyecto; comando directo: `dart format .`. Opcional: en CI o en hook ejecutar el mismo comando.
- **Black (Python):** `pip install black`. En `pyproject.toml`: `[tool.black]` con `line-length = 88`, `include = '\.pyi?$'`, `exclude` si aplica. Comando: `black .` o `black src/`.
- **google-java-format (Java):** Plugin para Gradle/Maven (ej. spotless con formatter google-java-format) o usar el JAR del CLI. Configuración típica: estilo Google por defecto; comando para formatear todo: `./gradlew spotlessApply` (con Spotless) o invocar el formatter sobre los fuentes.

### Pre-commit hooks

- **Husky + lint-staged (npm):** `npm install -D husky lint-staged`, luego `npx husky init`. Crear `.husky/pre-commit` con contenido `npx lint-staged`. En `package.json` añadir:

```json
"lint-staged": {
  "*.{js,ts,tsx}": ["eslint --fix", "prettier --write"],
  "*.{json,md}": ["prettier --write"]
}
```

En cada commit se ejecutará lint-staged sobre los archivos staged.

- **pre-commit (Python):** `pip install pre-commit`. Crear `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 24.1.0
    hooks:
      - id: black
  - repo: https://github.com/pycqa/pylint
    rev: v3.0.0
    hooks:
      - id: pylint
```

Ejecutar `pre-commit install` para instalar el hook en `.git/hooks/pre-commit`. Los hooks se ejecutan en cada commit.

- **Git hooks manuales:** Los hooks están en `.git/hooks/` (ej. `pre-commit`). Crear el archivo (sin extensión), contenido ejemplo: `#!/bin/sh` y `npm run lint && npm test`. Dar permisos: `chmod +x .git/hooks/pre-commit`. No versionar `.git/hooks`; documentar en README o usar Husky/pre-commit para que el equipo los instale.

## Configuración compartida

- Reglas de ESLint, Prettier, etc. en el repositorio (archivos de config) para que todo el equipo use las mismas.
- Umbrales de SonarQube (cobertura, duplicación, bugs) definidos en el proyecto y aplicados en CI; en local SonarLint usa las mismas reglas cuando está conectado al servidor.

Invertir en estas herramientas en local reduce deuda técnica y evita que lleguen a main código que luego falle en CI o en revisión.

---

[← Volver al README principal](../../README.md)
