# Pautas de colaboración en proyectos

> Documento vivo. Define cómo trabaja Raven Mind en sus proyectos: flujo de trabajo, branching, convención de commits, pull requests, tags, releases, CI/CD y manejo de incumplimientos.
>
> **Origen**: este documento se basa en el "Informe de Git y GitHub Raven Mind" aprobado por el equipo.

## Índice

1. [Introducción](README.md#1-introducción)
2. [Flujo de trabajo](README.md#2-flujo-de-trabajo)
3. [Modelo de branching](README.md#3-modelo-de-branching)
   - [3.1 Git Flow](README.md#31-git-flow)
   - [3.2 Trunk-based](README.md#32-trunk-based)
   - [3.3 Cuál modelo escoger y qué prácticas seguir](README.md#33-cuál-modelo-escoger-y-qué-prácticas-hay-que-seguir)
4. [Convención de commits](README.md#4-convención-de-commits)
   - [4.1 Aspectos a tener en cuenta](README.md#41-aspectos-a-tener-en-cuenta)
5. [Pull requests](README.md#5-pull-requests)
6. [Tags y releases](README.md#6-tags-y-releases)
7. [CI/CD y quality gates](README.md#7-cicd-y-quality-gates)
8. [Incumplimiento y excepciones](README.md#8-incumplimiento-y-excepciones)

---

## 1. Introducción

Nadie discute las ventajas de usar Git y GitHub para versionar proyectos y trazar cambios de manera ordenada y eficaz. Sin embargo, el uso indiscriminado de estas herramientas puede conducir a conflictos que interfieren con el propósito principal para el que fueron creadas: la gestión organizada de un proyecto.

Con el objetivo de evitar que se presenten estas situaciones, este documento establece las pautas y requisitos que se seguirán al utilizar Git y GitHub dentro de los flujos de trabajo de Raven Mind.

## 2. Flujo de trabajo

El equipo trabaja con un **repositorio principal único** del que una persona (el **Maintainer**) es la responsable. El resto del equipo, dado que aún no tiene un manejo avanzado de GitHub, no incide directamente sobre el repositorio principal, sino que trabaja desde **forks personales** y propone cambios mediante **Pull Requests** desde esos forks.

> Este flujo cambiará una vez todos los miembros sean capaces de manejar GitHub adecuadamente.

Este modelo es el oficial de GitHub para proyectos open source y resulta adecuado para equipos pequeños donde una sola persona concentra la gobernanza del repo.

### Responsabilidades del Maintainer

- Es el **Owner** del repositorio principal.
- Acepta, comenta, solicita cambios o rechaza los PRs abiertos por el resto del equipo.
- Mantiene la rama `main` (y `develop` si se usa Git Flow) limpia, actualizada y desplegable.
- Crea las **tags** y las **releases** exclusivamente en GitHub (nunca de forma local).
- Protege las ramas principales: ningún push directo, todo entra por PR.
- Acompaña al equipo en el uso de GitHub (revisiones, dudas, onboarding).

### Flujo paso a paso para un Contributor

1. **Fork del repositorio** (una sola vez): desde la UI de GitHub, cada Contributor hace fork de `empresa/repo-principal` hacia su cuenta personal. El fork queda en `https://github.com/<usuario>/<repo>`.
2. **Clonar el fork, no el repositorio principal**: se clona el fork propio:

   ```bash
   git clone https://github.com/<usuario>/<repo>.git
   ```

   Se añade el remoto `upstream` apuntando al repo principal:

   ```bash
   git remote add upstream https://github.com/empresa/repo-principal.git
   ```

3. **Mantener el fork actualizado**: antes de empezar una tarea, sincronizar con `upstream`:

   ```bash
   git checkout main
   git pull --rebase upstream main
   git push origin main
   ```

   Esto evita trabajar sobre código desactualizado y reduce conflictos en el PR.
4. **Crear una rama para el cambio**: el nombre de la rama refleja el tipo de cambio siguiendo la convención definida en la [sección 4](#4-convención-de-commits). Por ejemplo: `feat/login-google`, `fix/null-pointer-on-upload`.
5. **Trabajar, commitear y pushear al fork propio**: commits en la rama local y push al fork.
6. **Abrir un Pull Request hacia el repositorio principal**: desde la UI de GitHub, en la página del fork, abrir un PR hacia el repositorio principal, dar la información necesaria en el PR (descripción, screenshots, issue asociado) y pedir revisión al Maintainer.
7. **Atender la revisión**: en caso de solicitud de cambios por parte del Maintainer, aplicar cambios en la misma rama del fork propio y hacer push adicional; el PR se actualiza automáticamente. Una vez aprobado, el Maintainer hace el merge.
8. **Limpieza**: borrar la rama del fork propio tras el merge y sincronizar el fork con `upstream` para la próxima tarea.

## 3. Modelo de branching

El modelo de branching es de gran importancia. Especifica cómo deben ser estructuradas las ramas y qué tipo de commits pueden pertenecer a cada una.

Existen **dos modelos principales** con los que trabajaremos: **Git Flow** y **Trunk-based**.

### 3.1. Git Flow

Git Flow es una convención que se sigue para la estructura de ramas de un repositorio. Divide el repositorio en **2 ramas principales**: `main` y `develop`.

- En `main` solo puede haber commits listos para producción, es decir, versiones liberables.
- En `develop` se acumulan los commits listos para la próxima versión.

Para trabajar en distintos aspectos del repositorio, Git Flow propone la creación de diversas sub-ramas, cada una con su propio conjunto de reglas:

| Tipo de rama | Nace de | Propósito | Merge a |
| --- | --- | --- | --- |
| `feature/*` | `develop` | Trabajar en una feature específica (ej. `feature/login`). | `develop` |
| `release/*` | `develop` | Validaciones adicionales antes de enviar a producción. | `main` y de vuelta a `develop` |
| `hotfix/*` | `main` | Corregir un error crítico en producción. | `main` (con nuevo tag) y `develop` |

#### Ciclo de trabajo resumido

1. Trabajo en `feature/x` → merge a `develop`.
2. Repetir hasta tener varias features listas.
3. Cortar `release/1.4.0` desde `develop` → pulir detalles → merge a `main` (con su tag) y de vuelta a `develop`.
4. Si en `main` aparece un bug crítico → `hotfix/y` → merge a `main` (nuevo tag) y a `develop`.
5. Repetir.

> Para mayor claridad del modelo Git Flow recomendamos: [https://www.youtube.com/watch?v=Aa8RpP0sf-Y](https://www.youtube.com/watch?v=Aa8RpP0sf-Y).

### 3.2. Trunk-based

Es el modelo de branching más popular. Consiste en la creación de ramas (`feature/*`) por feature, implementadas en `main` mediante Pull Requests. Si existe un error crítico, se crean las respectivas ramas `hotfix/*` y se realizan merges mediante PRs sin mucha revisión.

### 3.3. Cuál modelo escoger y qué prácticas hay que seguir

La elección del modelo depende de la **complejidad del proyecto**:

- **Proyecto simple**, con actualizaciones regulares y donde la revisión granular del código no sea necesaria → **Trunk-based**.
- **Proyecto complejo**, con actualizaciones poco frecuentes, donde el código debe revisarse en detalle antes de subir a producción y la trazabilidad es vital → **Git Flow**.

#### Reglas comunes (aplican a ambos modelos)

- En `main` solo deben haber commits con releases. El código que se encuentra en `main` siempre debe ser apto para producción. Esto significa que no pueden haber commits atómicos ni que implementen cambios a medias.
- Las únicas ramas que **siempre** deben existir en el repositorio son `main` y `develop`. El resto de ramas (`feature/*`, `release/*`, `hotfix/*`) deben ser **borradas** una vez se haya terminado de trabajar en ellas, es decir, después de un PR exitoso o un merge con otra rama.
- Las convenciones de ramas según el modelo de branching elegido se deben seguir. Cualquier rama creada que no siga este modelo será eliminada, después de notificar al autor, del repositorio del proyecto. De esta manera se evitan confusiones y problemas de organización.

## 4. Convención de commits

Se seguirá el **formato convencional de commits flexibilizado**. Cada mensaje de commit debe cumplir con la siguiente estructura:

```text
type(optional scope): short description

Optional body

Optional footer
```

### `type`: tipo del commit

| Tipo | Significado |
| --- | --- |
| `feat` | Nueva funcionalidad visible para el usuario. |
| `fix` | Corrección de bug. |
| `docs` | Solo documentación. |
| `refactor` | Cambio interno que no altera comportamiento ni API. |
| `test` | Agregado o ajuste de tests. |
| `chore` | Tareas de mantenimiento (deps, build, CI, versionado, release). |

### `(optional scope)`

Información corta, opcional y adicional para explicar el alcance del commit. Generalmente no se usa, a no ser que el tipo sea `chore`.

### `short description`

Descripción corta sobre qué es lo que hace el commit. Debe ser escrita en **imperativo en inglés** (`add`, no `added`, por ejemplo).

### `Optional body`

Descripción opcional detallada sobre lo que hace el commit.

### `Optional footer`

Metadatos opcionales del commit.

### Ejemplo válido de un commit

```text
feat(auth): add OAuth2 login with Google provider

Users can now sign in using their Google account instead of email
and password. The flow validates the id_token against Google's
JWKS endpoint and provisions a local user on first login.

- Add `googleOAuthProvider` config in env.example
- Store provider + external_id in users table
- Add `POST /auth/google` endpoint
- Map JWT claims to local user claims

Refs: #142
Closes: #138
BREAKING CHANGE: the legacy `POST /auth/email` endpoint now
requires the `password_hash` field to be sent in the body header.
Clients must update to the new auth flow.
```

### 4.1. Aspectos a tener en cuenta

- La primera línea del mensaje del commit empieza en **minúscula**.
- Después de los dos puntos (`:`) se inicia en **minúscula**.
- Al final de la primera línea **no** se utiliza punto final.
- Referenciar issues con `#` (ej. `Closes: #42`).
- Si un commit implementa un cambio drástico en el proyecto que puede romper otros componentes funcionales, referenciarlo en el footer con `BREAKING CHANGE:`.
- El commit de merge que se realiza en `main` después de un PR se etiqueta como `chore(release): vX.Y.Z`.

## 5. Pull requests

Los Pull Requests deben ser:

- **Pequeños**.
- **Enfocados** en un solo cambio.
- Con **poco código por revisar** (idealmente **menos de 400 líneas**).

Todo PR debe contar con una **descripción obligatoria** en la cual se dé información sobre:

- El **contexto o problema** que se resuelve.
- Los **cambios principales**.
- **Cómo probarlo** (pasos, comandos, etc.).
- **Screenshots** si hay cambios en la UI.
- El **issue o ticket** asociado, si está basado en uno.

### Reglas de aprobación

- **Mínimo una persona** debe efectuar el rol de reviewer y aprobar el código.
- **Si el PR no es compilable, se rechazará**.
- **Si no se sigue la convención de commits y de branching, se rechazará**.
- **Si el código tiene demasiados conflictos, se rechazará**.

Cada autor de un PR debe asegurarse de que su código no cuente con conflictos con la rama `main`, o que cuente con la menor cantidad posible.

## 6. Tags y releases

El **Project Manager** se encargará de gestionar las tags y las releases en GitHub, **nunca localmente**. Está prohibido crear tags locales y pushearlas con `git push --tags`. Debe haber un tag por cada release y deben coincidir en nombre.

### Versionado SemVer estricto

Se utilizará **SemVer** (Semantic Versioning) estricto. Cada versión se referencia con la estructura `vMAJOR.MINOR.PATCH`:

- **MAJOR**: número que identifica a la versión principal; se entiende como la versión que es **incompatible** con la anterior.
- **MINOR**: número que identifica a una **funcionalidad implementada** que es **compatible** con la última versión MAJOR.
- **PATCH**: número que indica un **arreglo realizado** compatible con la última versión MAJOR.
- **Pre-release**: se indica al final de la etiqueta con `-rc.VERSION` o `-alpha.VERSION` (ej. `v1.0.0-rc.1`).

### Contenido de cada release

- **Título**: `vX.Y.Z`.
- **Notas**: pueden ser autogeneradas. Herramientas recomendadas: **release-please** o **standard-version**.
- Marcar si es una **latest release** o una **pre-release**.

### Reglas adicionales

- No se pueden **reutilizar tags** ya publicadas: cada tag corresponde a una versión específica.
- No se permite **modificar tags** ya existentes una vez que se encuentran asociadas a una versión en producción.

## 7. CI/CD y quality gates

Habrá herramientas como **lints** y **formats** ejecutadas en GitHub de manera automática para la revisión de código y la detección temprana de errores. Estas herramientas se implementarán mediante **GitHub Actions**, por lo que se recomienda encarecidamente el aprendizaje de esta funcionalidad de GitHub.

> Los workflows de CI/CD por proyecto se definirán caso por caso, pero como mínimo todo repositorio de Raven Mind debe tener linter y, cuando aplique, suite de tests ejecutándose en cada PR.

## 8. Incumplimiento y excepciones

En caso tal de que un miembro del equipo insista en saltarse las normativas especificadas en este documento, deberá **justificarlo de manera escrita** mediante una carta al Project Manager, o **verbalmente** ante una reunión con el equipo.

Cualquier evasión de reglas que no haya sido notificada previamente será motivo de **rechazo de Pull Requests** como fue especificado previamente.
