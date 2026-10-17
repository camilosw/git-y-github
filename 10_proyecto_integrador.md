# Tema 10: Proyecto integrador — Todo junto

> **Objetivo:** usar todo lo que aprendiste en un proyecto nuevo, desde cero, esta vez decidiendo tú qué comandos usar.
>
> **Antes de empezar:** este tema **no** usa `mi-proyecto`. Ubícate en la carpeta donde están `mi-proyecto` y `mi-proyecto-ana` (no dentro de ninguna de las dos), para no crear un repositorio dentro de otro.

Este tema no trae nada nuevo. Vas a hacer el recorrido completo en un mini proyecto PHP llamado `proyecto-final`:

- Crear un repositorio ([tema 2](02_primer_repositorio.md))
- El ciclo add/commit ([tema 3](03_ciclo_basico.md))
- Historial y `.gitignore` ([tema 4](04_historial.md))
- Corregir errores ([tema 5](05_deshacer.md))
- Ramas y conflictos ([tema 6](06_ramas.md))
- GitHub, push y README ([tema 7](07_github.md))
- Issues y Pull Requests ([tema 8](08_colaboracion.md))

## Cómo hacer este tema

A diferencia de los temas anteriores, **cada paso te dice qué lograr, no cómo**. Intenta hacerlo con lo que sabes, apoyándote en `git status` y en las sugerencias de Git. Los comandos de cada paso están escondidos en **"Ver los comandos"**: ábrelos para comparar cuando termines, o si te atascas de verdad. Si necesitas repasar algo, vuelve al tema correspondiente.

Al terminar, tu repositorio en GitHub va a tener:

- 5 archivos: `inicio.php`, `funciones.php`, `utilidades.php`, `.gitignore` y `README.md`
- Un historial de commits con mensajes claros, incluido un conflicto resuelto
- Un issue cerrado
- Un Pull Request aceptado (merged)

---

## Paso 1: Crear el repositorio

Crea una carpeta `proyecto-final`, entra en ella y conviértela en un repositorio Git. Verifica que Git diga `No commits yet`.

<details>
<summary>Ver los comandos</summary>

```bash
mkdir proyecto-final
cd proyecto-final
git init
git status
```

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

</details>

## Paso 2: Primer archivo y primer commit

Crea un archivo `inicio.php` con este contenido y guárdalo en un commit con un mensaje descriptivo:

```php
<?php
echo "Bienvenido al proyecto final";
?>
```

<details>
<summary>Ver los comandos</summary>

Después de crear el archivo con el editor:

```bash
git status
git add inicio.php
git commit -m "Agrega archivo inicio.php con mensaje de bienvenida"
```

</details>

## Paso 3: Segundo archivo, con un error para corregir

Crea un archivo `funciones.php` con este contenido:

```php
<?php
function sumar($a, $b) {
    return $a + $b;
}

echo "3 + 5 = " . sumar(3, 5);
?>
```

Guárdalo en un commit con este mensaje, **con errores a propósito**: `Agrga funciones.php con funcion sumr`. Después corrige el mensaje sin crear un commit nuevo. Verifica en el historial que el mensaje con errores ya no aparece.

<details>
<summary>Ver los comandos</summary>

```bash
git add funciones.php
git commit -m "Agrga funciones.php con funcion sumr"
git commit --amend -m "Agrega funciones.php con función sumar"
git log --oneline
```

```
b2c3d4e Agrega funciones.php con función sumar
a1b2c3d Agrega archivo inicio.php con mensaje de bienvenida
```

Se puede usar `--amend` porque este commit todavía no está en GitHub.

</details>

## Paso 4: .gitignore

Haz que Git ignore los archivos `.log`, `.tmp` y `.DS_Store`, y guarda esa configuración en el historial. Comprueba que funciona creando un archivo `prueba.log`, y después bórralo.

<details>
<summary>Ver los comandos</summary>

Crea `.gitignore` con este contenido:

```
*.log
*.tmp
.DS_Store
```

```bash
git add .gitignore
git commit -m "Agrega .gitignore para ignorar archivos temporales"
echo "prueba" > prueba.log
git status
rm prueba.log
```

`git status` tiene que decir `working tree clean`: `prueba.log` no aparece.

</details>

## Paso 5: Subirlo a GitHub

Crea en GitHub un repositorio vacío llamado `proyecto-final` (público, sin README, sin `.gitignore`, sin licencia), conéctalo con tu repositorio local y sube todo. Verifica en GitHub que aparecen los 3 archivos y los 3 commits.

<details>
<summary>Ver los comandos</summary>

En GitHub: **+** → **New repository** → nombre `proyecto-final` → **Create repository**. Después (con tu usuario; con SSH la URL es `git@github.com:tu-usuario/proyecto-final.git`):

```bash
git remote add origin https://github.com/tu-usuario/proyecto-final.git
git push -u origin main
```

</details>

## Paso 6: README

Agrega un `README.md` con este contenido, guárdalo y súbelo. Antes de subirlo, fíjate qué dice `git status` sobre GitHub.

```markdown
# Proyecto Final

Proyecto integrador del curso de Git y GitHub.

## Archivos

- inicio.php: mensaje de bienvenida
- funciones.php: funciones matemáticas
- .gitignore: archivos que Git ignora
```

<details>
<summary>Ver los comandos</summary>

```bash
git add README.md
git commit -m "Agrega README con descripción del proyecto"
git status
git push
```

Antes del push, `git status` dice `Your branch is ahead of 'origin/main' by 1 commit`.

</details>

## Paso 7: Un issue y su rama

Crea en GitHub un issue: **"Agregar archivo de utilidades"**, que pida crear `utilidades.php` con funciones para saludar y despedir. Resuélvelo en una rama propia con este archivo y sube la rama:

```php
<?php
function saludar($nombre) {
    return "Hola, " . $nombre . "!";
}

function despedir($nombre) {
    return "Adiós, " . $nombre . ". ¡Hasta la próxima!";
}

echo saludar("Sandra");
echo despedir("Sandra");
?>
```

<details>
<summary>Ver los comandos</summary>

En GitHub: **Issues** → **New issue** → título y descripción → **Create**. Es el issue #1. Después:

```bash
git switch -c issue-1-utilidades
git add utilidades.php
git commit -m "Agrega utilidades.php con funciones saludar y despedir (issue #1)"
git push -u origin issue-1-utilidades
```

</details>

## Paso 8: Pull Request, revisión y merge

Abre un Pull Request para la rama, de forma que **el issue se cierre solo** al aceptarlo. Revisa los cambios en GitHub, deja al menos un comentario en una línea, y acepta el PR. Comprueba que el issue quedó cerrado.

<details>
<summary>Ver los pasos</summary>

1. En GitHub: **Compare & pull request**
2. Título: `Agrega archivo de utilidades`. Descripción: `Closes #1` (en inglés: "Resuelve #1" no cierra el issue)
3. **Create pull request**
4. **Files changed** → **+** en una línea → escribir el comentario → **Add single comment**
5. **Merge pull request** → **Confirm merge**
6. En **Issues** → **Closed** aparece el issue #1

</details>

## Paso 9: Sincronizar tu computadora

Trae a tu `main` local el merge que se hizo en GitHub y borra la rama que ya no hace falta. Antes de traer los cambios, piensa: ¿por qué `git status` dice que estás al día?

<details>
<summary>Ver los comandos</summary>

```bash
git switch main
git pull
git branch -d issue-1-utilidades
```

`git status` dice "up to date" antes del pull porque no se conecta a GitHub: compara con lo que Git sabía en tu último push.

</details>

## Paso 10: Un conflicto

Vas a mejorar el mensaje de bienvenida de dos formas distintas, en dos ramas, y resolver el conflicto.

1. En una rama nueva llamada `mejorar-bienvenida`, cambia la línea del `echo` de `inicio.php` por `echo "Te damos la bienvenida al proyecto final";` y haz commit.
2. En `main`, cambia **la misma línea** por `echo "Bienvenida al proyecto final de Git";` y haz commit.
3. Une `mejorar-bienvenida` en `main`. Resuelve el conflicto con el texto que prefieras, termina el merge, borra la rama y sube `main` a GitHub.

<details>
<summary>Ver los comandos</summary>

```bash
git switch -c mejorar-bienvenida
# editar inicio.php: echo "Te damos la bienvenida al proyecto final";
git add inicio.php
git commit -m "Cambia el mensaje de bienvenida"

git switch main
# editar inicio.php: echo "Bienvenida al proyecto final de Git";
git add inicio.php
git commit -m "Personaliza el mensaje de bienvenida"

git merge mejorar-bienvenida
```

```
Auto-merging inicio.php
CONFLICT (content): Merge conflict in inicio.php
Automatic merge failed; fix conflicts and then commit the result.
```

`inicio.php` queda así:

```
<?php
<<<<<<< HEAD
echo "Bienvenida al proyecto final de Git";
=======
echo "Te damos la bienvenida al proyecto final";
>>>>>>> mejorar-bienvenida
?>
```

Edítalo para dejar una sola línea de `echo`, sin marcas (por ejemplo, `echo "Te damos la bienvenida al proyecto final de Git";`), y después:

```bash
git add inicio.php
git commit --no-edit
git branch -d mejorar-bienvenida
git push
```

</details>

---

## ✅ Comprobación final

**En la terminal:**

```bash
ls
```

```
README.md  funciones.php  inicio.php  utilidades.php
```

(`.gitignore` no aparece porque es un archivo oculto. Usa `ls -a` para verlo.)

```bash
git log --oneline --graph
```

Algo parecido a esto (los códigos y el orden de algunas líneas pueden variar):

```
*   c9d0e1f Merge branch 'mejorar-bienvenida'
|\
| * a7b8c9d Cambia el mensaje de bienvenida
* | b8c9d0e Personaliza el mensaje de bienvenida
|/
*   f6a7b8c Merge pull request #2 from tu-usuario/issue-1-utilidades
|\
| * e5f6a7b Agrega utilidades.php con funciones saludar y despedir (issue #1)
|/
* d4e5f6a Agrega README con descripción del proyecto
* c3d4e5f Agrega .gitignore para ignorar archivos temporales
* b2c3d4e Agrega funciones.php con función sumar
* a1b2c3d Agrega archivo inicio.php con mensaje de bienvenida
```

```bash
git status
```

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**En GitHub:**

- Aparecen los 5 archivos: `inicio.php`, `funciones.php`, `utilidades.php`, `.gitignore`, `README.md`
- El README se ve como portada
- El issue #1 está **cerrado**
- El Pull Request #2 está **merged**, con tu comentario de revisión
- El historial muestra todos los commits, incluido el merge del conflicto

## Para seguir practicando

Si quieres más práctica, prueba estos retos sobre `proyecto-final`, sin guía:

- Haz un commit que agregue una función a `funciones.php`, súbelo, y después deshazlo **sin borrar historial**.
- Clona `proyecto-final` en otra carpeta, como hiciste con Ana, y provoca un push rechazado.
- Recupera en tu carpeta la primera versión de `inicio.php`, míralo, y vuelve a la versión actual.

---

## ¡Felicitaciones!

Si llegaste hasta aquí, ya sabes:

- Crear repositorios y guardar commits con mensajes claros
- Elegir qué cambios van en cada commit
- Recorrer el historial y ver qué cambió en cada momento
- Deshacer errores en cualquier etapa
- Trabajar con ramas y resolver conflictos
- Subir tu código a GitHub y descargar repositorios con `git clone`
- Organizar el trabajo con issues y revisar cambios con Pull Requests
- Combinar tu trabajo con el de otras personas

Son las bases que usan los equipos de desarrollo en todo el mundo.
