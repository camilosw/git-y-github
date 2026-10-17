# Tema 8: Issues y Pull Requests — Organizar y revisar el trabajo

> **Objetivo:** usar issues para organizar el trabajo y Pull Requests para proponer cambios y revisarlos antes de aceptarlos.
>
> **Antes de empezar:** tu repositorio `mi-proyecto` tiene que estar subido a GitHub ([tema 7](07_github.md)), y en la terminal tienes que estar dentro de `mi-proyecto`, en la rama `main`.

## Issues y Pull Requests

GitHub no solo guarda código: también tiene herramientas para organizar el trabajo, aunque trabajes sola.

Piensa en un proyecto como una lista de tareas:

- Los **issues** son las tareas: "hay que agregar tal archivo", "hay que corregir tal error".
- Los **Pull Requests** (PR) son la forma de decir "terminé esta tarea; revisa los cambios antes de aceptarlos".

El flujo completo es: crear un issue, crear una rama, hacer los cambios, abrir un PR, revisarlo y aceptarlo. Es la forma de trabajo estándar en equipos de desarrollo, así que te va a servir siempre.

## Issues: tareas y pendientes

Un **issue** es como un ticket. Sirve para:

- Reportar un error ("el archivo X no funciona")
- Proponer una mejora ("agregar un archivo de utilidades")
- Anotar algo pendiente ("falta agregar validación")

GitHub numera los issues automáticamente (#1, #2, #3…) y ese número sirve para mencionarlos.

### ✍️ Tu turno: crear un issue

1. Entra a tu repositorio `mi-proyecto` en GitHub
2. Haz clic en la pestaña **Issues**
3. Haz clic en **New issue**
4. **Título:** `Agregar archivo de utilidades`
5. **Descripción:** `Crear un archivo utilidades.php con una función que salude por nombre`
6. Haz clic en **Create** (en algunas versiones de GitHub dice **Submit new issue**)

Se crea el issue **#1**.

## Una rama para cada issue

La buena práctica es resolver cada issue en su propia rama. Así los cambios quedan ordenados y separados de `main` hasta que estén listos. Se suele poner el número del issue en el nombre de la rama.

### ✍️ Tu turno: resolver el issue en una rama

Asegúrate de estar en `main` y crea la rama:

```bash
git switch main
git switch -c issue-1-utilidades
```

```
Switched to a new branch 'issue-1-utilidades'
```

Crea un archivo `utilidades.php` con este contenido:

```php
<?php
function saludar($nombre) {
    return "Hola, " . $nombre . "!";
}

echo saludar("Sandra");
?>
```

Guárdalo en el historial. En el mensaje mencionamos el issue con `#1`:

```bash
git add utilidades.php
git commit -m "Agrega archivo de utilidades (issue #1)"
```

```
[issue-1-utilidades 463ffcc] Agrega archivo de utilidades (issue #1)
 1 file changed, 7 insertions(+)
 create mode 100644 utilidades.php
```

Sube la rama a GitHub. Como es una rama nueva, la primera vez también lleva `-u`:

```bash
git push -u origin issue-1-utilidades
```

```
...
remote: Create a pull request for 'issue-1-utilidades' on GitHub by visiting:
remote:      https://github.com/tu-usuario/mi-proyecto/pull/new/issue-1-utilidades
...
To https://github.com/tu-usuario/mi-proyecto.git
 * [new branch]      issue-1-utilidades -> issue-1-utilidades
```

GitHub incluso te da un enlace para crear el Pull Request.

## Pull Requests: proponer cambios para revisar

Un **Pull Request** es una solicitud para unir los cambios de una rama en `main`. En vez de hacer `git merge` en tu computadora (como en el tema 6), abres un PR en GitHub para que los cambios se puedan **revisar** antes de aceptarlos.

### ✍️ Tu turno: abrir el Pull Request

1. Entra a tu repositorio en GitHub
2. Arriba aparece un cartel amarillo con tu rama y el botón **Compare & pull request**: haz clic ahí. (Si no aparece, ve a la pestaña **Pull requests** → **New pull request** y elige la rama `issue-1-utilidades`.)
3. **Título:** `Agrega archivo de utilidades`
4. **Descripción:** `Closes #1`
5. Haz clic en **Create pull request**

> **¿Por qué `Closes #1`?** Si la descripción de un PR dice `Closes #1`, GitHub cierra el issue #1 automáticamente cuando se acepta el PR. **La palabra tiene que estar en inglés:** GitHub solo reconoce `close`, `closes`, `closed`, `fix`, `fixes`, `fixed`, `resolve`, `resolves` o `resolved`. Si escribes "Resuelve #1", el PR menciona el issue pero no lo cierra.

> 🤔 **Antes de mirar, piensa:** el issue es el #1. ¿Qué número crees que le asignó GitHub a tu Pull Request?

<details>
<summary>Ver la respuesta</summary>

El **#2**. Issues y Pull Requests comparten la misma numeración en cada repositorio, así que el PR toma el siguiente número libre.

</details>

### Revisar el PR

Revisar antes de aceptar es el objetivo de un PR. En la página del PR hay varias pestañas:

- **Conversation:** comentarios y discusión
- **Commits:** los commits incluidos
- **Files changed:** los archivos modificados, con las líneas agregadas (verde) y quitadas (rojo), como en `git diff`

#### ✍️ Tu turno: revisar como lo haría una compañera

1. Abre la pestaña **Files changed** y revisa que `utilidades.php` aparezca completo en verde (todo es nuevo).
2. Pasa el mouse sobre la línea `echo saludar("Sandra");`: aparece un botón **+** azul a la izquierda. Haz clic y escribe un comentario, por ejemplo: `¿Este echo de prueba hace falta en el archivo final?`. Haz clic en **Add single comment**.
3. Vuelve a la pestaña **Conversation**: tu comentario aparece ahí, ligado a esa línea.

En un equipo, así es como alguien revisa tu código y te hace preguntas o sugerencias antes de aceptarlo. Si hubiera que cambiar algo, harías un commit nuevo en la misma rama y un `git push`: el PR se actualiza solo.

### Aceptar el PR (merge)

#### ✍️ Tu turno

1. En la página del PR, haz clic en **Merge pull request**
2. Haz clic en **Confirm merge**

El PR queda como **Merged** y el issue #1 se cierra solo. GitHub también te ofrece un botón **Delete branch** para borrar la rama en GitHub: puedes usarlo, ya no la necesitas.

## Sincronizar tu computadora: git pull

El merge se hizo en GitHub, pero la `main` de tu computadora todavía no lo tiene. `git pull` trae los cambios de GitHub.

### ✍️ Tu turno: traer los cambios

```bash
git switch main
```

```
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

> 🤔 **Piensa:** Git dice que tu `main` está al día con la de GitHub. Pero en GitHub acabas de hacer un merge. ¿Cómo puede ser?

<details>
<summary>Ver la respuesta</summary>

`git status` y `git switch` no se conectan a internet. Comparan tu `main` con lo que Git **sabía** de GitHub la última vez que habló con él (en tu último push o pull). Hasta que no le preguntes, Git no se entera de lo que pasó allá. Eso es lo que hace `git pull`.

</details>

```bash
git pull
```

Deberías ver algo parecido a esto:

```
...
From https://github.com/tu-usuario/mi-proyecto
   14892ce..cdb850d  main       -> origin/main
Updating 14892ce..cdb850d
Fast-forward
 utilidades.php | 7 +++++++
 1 file changed, 7 insertions(+)
 create mode 100644 utilidades.php
```

Ahora tu `main` local tiene `utilidades.php`. Como los cambios ya están en `main`, borra la rama local:

```bash
git branch -d issue-1-utilidades
```

## Resumen del flujo completo

```
1. Crear un ISSUE en GitHub      → qué hay que hacer
            ↓
2. Crear una RAMA local          → git switch -c issue-1-nombre
            ↓
3. Hacer CAMBIOS y COMMIT        → git add + git commit
            ↓
4. SUBIR la rama                 → git push -u origin nombre-rama
            ↓
5. Abrir un PULL REQUEST         → en GitHub, con "Closes #N"
            ↓
6. REVISAR y hacer MERGE         → en GitHub, "Merge pull request"
            ↓
7. SINCRONIZAR                   → git switch main + git pull
```

## Resumen de comandos nuevos

| Comando | Qué hace |
|---|---|
| `git push -u origin nombre-rama` | Sube una rama nueva a GitHub |
| `git pull` | Trae a tu computadora los cambios de GitHub |

## 🧩 Desafío

Haz el flujo completo tú sola, sin mirar los pasos de arriba:

- Crea un issue: **"Agregar función de despedida"**, que pida agregar a `utilidades.php` una función `despedir($nombre)`.
- Resuélvelo en una rama, abre un PR que cierre el issue automáticamente, acéptalo y sincroniza tu computadora.

Antes de empezar: ¿qué número va a tener el issue? ¿Y el PR?

<details>
<summary>Ver una solución</summary>

El issue va a ser el **#3** y el PR el **#4** (el #1 y el #2 ya los usaste).

1. En GitHub: **Issues** → **New issue**, con el título y la descripción → **Create**.
2. En la terminal:

   ```bash
   git switch main
   git pull
   git switch -c issue-3-despedida
   ```

   (El `git pull` al principio es un buen hábito: empiezas la rama desde la versión más nueva de `main`.)
3. Agrega a `utilidades.php`, antes de la línea `?>`, la función:

   ```php
   function despedir($nombre) {
       return "Adiós, " . $nombre . "!";
   }
   ```

4. Guarda y sube:

   ```bash
   git add utilidades.php
   git commit -m "Agrega función despedir (issue #3)"
   git push -u origin issue-3-despedida
   ```

5. En GitHub: **Compare & pull request**, con `Closes #3` en la descripción → **Create pull request** → revisa **Files changed** → **Merge pull request** → **Confirm merge**.
6. En la terminal:

   ```bash
   git switch main
   git pull
   git branch -d issue-3-despedida
   ```

</details>

## ✅ Comprueba que todo salió bien

**En GitHub:**

- Pestaña **Issues** → **Closed**: aparece "Agregar archivo de utilidades" como cerrado (y "Agregar función de despedida", si hiciste el desafío).
- Pestaña **Pull requests** → **Closed**: aparece "Agrega archivo de utilidades" con un ícono violeta que dice **Merged**.

**En la terminal:**

```bash
git log --oneline -3
```

Arriba tienen que aparecer el commit del merge que hizo GitHub y el de `utilidades.php` (si hiciste el desafío, los de la despedida van a estar más arriba todavía):

```
cdb850d Merge pull request #2 from tu-usuario/issue-1-utilidades
463ffcc Agrega archivo de utilidades (issue #1)
14892ce Agrega README con descripción del proyecto
```

```bash
git branch
```

```
* main
```

Y `git status` tiene que decir `working tree clean`.

---

**Siguiente:** [Tema 9: Trabajar en equipo](09_trabajo_en_equipo.md)
