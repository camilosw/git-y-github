# Tema 9: Trabajar en equipo — clone, pull y cambios simultáneos

> **Objetivo:** descargar un repositorio con `git clone`, y combinar tu trabajo con el de otra persona que sube cambios al mismo repositorio, incluso cuando las dos trabajaron al mismo tiempo.
>
> **Antes de empezar:** entra en `mi-proyecto`. Tiene que estar en `main`, sincronizado con GitHub ([tema 8](08_colaboracion.md)): `git status` tiene que decir `up to date with 'origin/main'` y `working tree clean`.

## Lo que cambia cuando hay más personas

Hasta ahora, todo lo que había en GitHub lo subiste tú. En un equipo, varias personas suben commits al mismo repositorio. Eso trae dos situaciones nuevas:

1. Alguien sube cambios y tú necesitas traerlos a tu computadora.
2. Dos personas trabajan **al mismo tiempo** y las dos quieren subir sus commits.

Para practicarlo sin otra persona, vamos a **simular una compañera**, Ana, con una segunda copia del repositorio en otra carpeta de tu computadora. Para Git es exactamente lo mismo que si Ana estuviera en otra computadora: las dos copias solo se comunican a través de GitHub.

## Una configuración antes de empezar

Cuando dos personas suben commits al mismo tiempo, `git pull` tiene que combinar los dos trabajos. Git necesita que le digas **cómo** quieres que lo haga; si no, se detiene con el error `Need to specify how to reconcile divergent branches`.

### ✍️ Tu turno

```bash
git config --global pull.rebase false
```

Esto le dice a Git: "cuando haya que combinar, haz un merge", igual que los merges del tema 6. Solo hay que configurarlo una vez.

## git clone: descargar un repositorio

`git clone` descarga un repositorio de GitHub completo, con todo su historial, y lo deja listo para trabajar. Es lo que vas a hacer cada vez que quieras trabajar en un proyecto que ya existe en GitHub: el tuyo en otra computadora, el de tu equipo o cualquier proyecto público.

### ✍️ Tu turno: clonar la copia de Ana

Sal de `mi-proyecto` a la carpeta de arriba y clona tu repositorio en una carpeta nueva llamada `mi-proyecto-ana`. Usa la misma URL que usaste en `git remote add` en el tema 7:

```bash
cd ..
git clone https://github.com/tu-usuario/mi-proyecto.git mi-proyecto-ana
```

```
Cloning into 'mi-proyecto-ana'...
...
Receiving objects: 100% (45/45), done.
```

(El último argumento, `mi-proyecto-ana`, es el nombre de la carpeta. Si no lo pones, Git usa el nombre del repositorio.)

Ahora tienes dos carpetas, una al lado de la otra:

```
mi-proyecto/       ← tu copia
mi-proyecto-ana/   ← la copia de Ana
```

Entra en la copia de Ana:

```bash
cd mi-proyecto-ana
ls
```

```
README.md  ayuda.php  contacto.php  hola.php  nuevo.php  utilidades.php
```

> 🤔 **Antes de ejecutarlo, piensa:** en el tema 7, para conectar tu repositorio con GitHub tuviste que usar `git remote add origin`. ¿Hace falta hacerlo en un repositorio clonado?

```bash
git remote -v
```

<details>
<summary>Ver lo que deberías ver</summary>

```
origin	https://github.com/tu-usuario/mi-proyecto.git (fetch)
origin	https://github.com/tu-usuario/mi-proyecto.git (push)
```

No hace falta: `git clone` ya deja configurado `origin` apuntando al repositorio del que clonaste, y la rama `main` vinculada con la de GitHub. Puedes hacer `git push` y `git pull` directamente.

</details>

### Darle a Ana su propio nombre

Los commits que hagas en esta carpeta saldrían con tu nombre, porque la configuración del tema 1 es `--global`. Para que el historial muestre claramente quién hizo qué, configura a Ana **solo en este repositorio**, usando `git config` **sin** `--global`:

```bash
git config user.name "Ana"
git config user.email "ana@ejemplo.com"
```

La configuración sin `--global` vale solo para el repositorio donde estás y tiene prioridad sobre la global. Tu carpeta `mi-proyecto` sigue usando tu nombre.

## Situación 1: alguien sube cambios y tú los traes

### ✍️ Tu turno: Ana sube un cambio

Siempre dentro de `mi-proyecto-ana`, Ana actualiza el email de contacto:

```bash
echo '<?php echo "Escríbenos a hola@ejemplo.com"; ?>' > contacto.php
git add contacto.php
git commit -m "Actualiza el email de contacto"
git push
```

```
...
To https://github.com/tu-usuario/mi-proyecto.git
   cdb850d..67e9a40  main -> main
```

### ✍️ Tu turno: traer el cambio de Ana

Vuelve a tu copia:

```bash
cd ../mi-proyecto
```

(`..` es la carpeta de arriba, así que `../mi-proyecto` es "subir un nivel y entrar en `mi-proyecto`".)

> 🤔 **Antes de ejecutarlo, piensa:** Ana ya subió su commit a GitHub. ¿Qué va a decir `git status` en tu copia? (Pista: el tema 8.)

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Dice "up to date" aunque GitHub tiene un commit nuevo. `git status` no se conecta a GitHub: compara con lo que Git sabía en tu último push o pull.

</details>

Trae los cambios:

```bash
git pull
```

```
...
Updating cdb850d..67e9a40
Fast-forward
 contacto.php | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Mira el último commit:

```bash
git log -1
```

```
commit 67e9a40...
Author: Ana <ana@ejemplo.com>
Date:   ...

    Actualiza el email de contacto
```

El commit de Ana está en tu historial, con su nombre.

## Situación 2: las dos trabajan al mismo tiempo

Ahora lo más común en un equipo: tú haces un commit, y mientras tanto Ana hace otro y lo sube primero.

### ✍️ Tu turno: dos commits a la vez

**1. Tú** (en `mi-proyecto`) amplías la ayuda y haces un commit, **sin subirlo todavía**:

```bash
echo '<?php echo "Página de ayuda: escríbenos si tienes dudas"; ?>' > ayuda.php
git add ayuda.php
git commit -m "Amplía el texto de ayuda"
```

**2. Ana**, mientras tanto, agrega una línea al README y la sube:

```bash
cd ../mi-proyecto-ana
git pull
echo "" >> README.md
echo "Hecho por Sandra y Ana." >> README.md
git add README.md
git commit -m "Agrega autoras al README"
git push
```

(Ana empieza con `git pull`, para trabajar sobre la versión más nueva. Es un buen hábito.)

**3. Tú** vuelves a tu copia e intentas subir tu commit:

```bash
cd ../mi-proyecto
```

> 🤔 **Antes de ejecutarlo, piensa:** tu `main` tiene un commit que GitHub no tiene (el tuyo), y la `main` de GitHub tiene un commit que tú no tienes (el de Ana). ¿Qué crees que va a pasar con `git push`?

```bash
git push
```

<details>
<summary>Ver lo que deberías ver</summary>

```
To https://github.com/tu-usuario/mi-proyecto.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/tu-usuario/mi-proyecto.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
```

GitHub **rechaza** el push. Si lo aceptara, tu `main` reemplazaría a la de GitHub y el commit de Ana se perdería. Git te protege de eso, y la sugerencia (`hint`) te dice qué hacer: primero `git pull`.

</details>

Es un mensaje que vas a ver seguido trabajando en equipo, y no es un error tuyo: solo significa "alguien subió algo antes que tú". La solución es traer sus cambios, combinarlos con los tuyos y volver a subir:

```bash
git pull --no-edit
```

(`--no-edit` usa el mensaje automático para el commit de merge, sin abrir el editor.)

```
...
Merge made by the 'ort' strategy.
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

Como tu commit y el de Ana tocaron archivos distintos, Git los combinó solo, con un commit de merge, como en el tema 6. Míralo dibujado:

```bash
git log --oneline --graph -4
```

```
*   262c1aa Merge branch 'main' of https://github.com/tu-usuario/mi-proyecto
|\
| * 6657aa9 Agrega autoras al README
* | 3647ddf Amplía el texto de ayuda
|/
* 67e9a40 Actualiza el email de contacto
```

Ahora sí, sube todo:

```bash
git push
```

```
...
To https://github.com/tu-usuario/mi-proyecto.git
   6657aa9..262c1aa  main -> main
```

> **¿Y si Ana y tú hubieran cambiado la misma línea?** Entonces `git pull` termina con un conflicto, igual que el `git merge` del tema 6, y se resuelve de la misma forma: editas el archivo, quitas las marcas, `git add`, `git commit --no-edit` y después `git push`. Lo vas a practicar en el desafío.

## Buenos hábitos para trabajar en equipo

- **`git pull` antes de empezar a trabajar.** Cuanto más nueva sea la versión de la que partes, menos combinaciones vas a necesitar.
- **Commits pequeños y `git push` seguido.** Si pasas días sin subir, aumentan las probabilidades de que alguien toque lo mismo que tú.
- **Para cambios más grandes, una rama y un Pull Request** (tema 8), así alguien los revisa antes de que lleguen a `main`.
- **Si te rechazan el push, no te asustes:** `git pull`, resuelve si hay conflicto, y `git push`.

## 🧩 Desafío

Provoca un conflicto entre Ana y tú, y resuélvelo:

- Ana cambia el email de `contacto.php` a `ana@ejemplo.com` y lo sube.
- Tú, sin traer antes sus cambios, cambias la **misma línea** a `sandra@ejemplo.com`, haces commit e intentas subirlo.
- Resuelve el conflicto dejando los dos emails, y sube el resultado.

<details>
<summary>Ver una solución</summary>

**Ana:**

```bash
cd ../mi-proyecto-ana
git pull
echo '<?php echo "Escríbenos a ana@ejemplo.com"; ?>' > contacto.php
git add contacto.php
git commit -m "Cambia el contacto al email de Ana"
git push
```

**Tú:**

```bash
cd ../mi-proyecto
echo '<?php echo "Escríbenos a sandra@ejemplo.com"; ?>' > contacto.php
git add contacto.php
git commit -m "Cambia el contacto al email de Sandra"
git push
```

El push es rechazado (`fetch first`), como antes. Trae los cambios:

```bash
git pull --no-edit
```

```
...
Auto-merging contacto.php
CONFLICT (content): Merge conflict in contacto.php
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
cat contacto.php
```

```
<<<<<<< HEAD
<?php echo "Escríbenos a sandra@ejemplo.com"; ?>
=======
<?php echo "Escríbenos a ana@ejemplo.com"; ?>
>>>>>>> 3168a1a40ec09fe966d0d86ba1a8896745939d28
```

Arriba está tu versión (`HEAD`); abajo, la que vino de GitHub (en lugar de un nombre de rama, Git muestra el hash del commit de Ana). Edita el archivo para que quede así, sin marcas:

```php
<?php echo "Escríbenos a sandra@ejemplo.com o a ana@ejemplo.com"; ?>
```

Y termina como en el tema 6, más el push:

```bash
git add contacto.php
git commit --no-edit
git push
```

Para que la copia de Ana también tenga la resolución:

```bash
cd ../mi-proyecto-ana
git pull
cd ../mi-proyecto
```

</details>

## ✅ Comprueba que todo salió bien

**En GitHub**, en la lista de commits de `mi-proyecto`, tienen que aparecer commits de Ana ("Actualiza el email de contacto", "Agrega autoras al README") y el commit de merge "Merge branch 'main' of …".

**En la terminal**, dentro de `mi-proyecto`:

```bash
git status
```

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

```bash
cat README.md
```

```
# Mi Proyecto

Este es mi primer proyecto con Git y GitHub.

Hecho por Sandra y Ana.
```

La carpeta `mi-proyecto-ana` ya no la vas a usar. Puedes borrarla desde el explorador de archivos o dejarla: no afecta a `mi-proyecto` ni a GitHub.

---

**Siguiente:** [Tema 10: Proyecto integrador](10_proyecto_integrador.md)
