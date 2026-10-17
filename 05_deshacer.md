# Tema 5: Deshacer cambios — restore, amend, revert

> **Objetivo:** aprender a deshacer errores en cada etapa: antes de `git add`, después de `git add`, en el último commit y en un commit viejo. También vas a ver y recuperar versiones anteriores de un archivo.
>
> **Antes de empezar:** entra en `mi-proyecto`. Tiene que tener los 5 commits del [tema 4](04_historial.md) y `git status` tiene que decir `working tree clean`.

## Equivocarse es parte del trabajo

En el tema 1 dijimos que con Git siempre puedes volver atrás. Este tema muestra cómo. El comando correcto depende de **dónde está el error**:

| ¿Dónde está el error? | Comando |
|---|---|
| Cambié un archivo y quiero descartar el cambio (no hice `git add`) | `git restore archivo` |
| Puse algo en la caja que no quería (hice `git add`) | `git restore --staged archivo` |
| El último commit tiene un error y todavía no lo sube a GitHub | `git commit --amend` |
| Quiero ver o recuperar cómo era un archivo antes | `git show` / `git restore --source` |
| Un commit viejo tiene un error (aunque ya lo haya subido) | `git revert` |

No hace falta memorizar la tabla: `git status` te sugiere varios de estos comandos en sus mensajes entre paréntesis.

## 1. Descartar cambios que no guardaste: git restore

Si modificaste un archivo y quieres dejarlo como estaba en el último commit, usa `git restore`.

### ✍️ Tu turno: romper y arreglar un archivo

Rompe `hola.php` a propósito, reemplazando todo su contenido:

```bash
echo 'ERROR' > hola.php
cat hola.php
```

```
ERROR
```

(`cat` muestra el contenido de un archivo.) Git detecta el cambio:

```bash
git status
```

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   hola.php

no changes added to commit (use "git add" and/or "git commit -a")
```

Fíjate en la sugerencia: `use "git restore <file>..." to discard changes`. Hazle caso:

```bash
git restore hola.php
cat hola.php
```

```
<?php echo "Hola mundo"; echo "Adiós mundo"; echo "Versión 3"; ?>
```

El archivo volvió a estar como en el último commit.

> ⚠️ **Cuidado:** `git restore` borra los cambios que no guardaste, y esos cambios **no se pueden recuperar**, porque nunca estuvieron en un commit. Antes de usarlo, mira con `git diff` qué vas a perder.

## 2. Sacar algo de la caja: git restore --staged

Si hiciste `git add` de algo que no querías incluir en el próximo commit, puedes sacarlo de la caja con `git restore --staged`. El archivo y tus cambios **no se pierden**: solo vuelven a la mesa.

### ✍️ Tu turno: un git add . de más

Crea un archivo de borrador y agrega todo sin mirar, un error muy común:

```bash
echo '<?php // borrador, no terminado ?>' > borrador.php
git add .
git status
```

```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   borrador.php
```

El borrador está en la caja y entraría en el próximo commit. Sácalo:

```bash
git restore --staged borrador.php
```

> 🤔 **Antes de ejecutarlo, piensa:** ¿el archivo `borrador.php` sigue existiendo? ¿Cómo lo va a mostrar `git status`?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	borrador.php

nothing added to commit but untracked files present (use "git add" to track)
```

El archivo sigue existiendo, intacto. Solo salió de la caja: volvió a ser un archivo sin seguimiento, como antes del `git add`.

</details>

Como no lo vamos a usar, bórralo:

```bash
rm borrador.php
```

## 3. Corregir el último commit: git commit --amend

Si te equivocaste en el **último** commit (un error en el mensaje, o te olvidaste de un archivo), puedes corregirlo con `--amend` ("enmendar"). Git reemplaza el último commit por uno corregido.

### ✍️ Tu turno: corregir un mensaje con errores

Crea una página de ayuda y haz el commit con un mensaje mal escrito, a propósito:

```bash
echo '<?php echo "Página de ayuda"; ?>' > ayuda.php
git add ayuda.php
git commit -m "Agrga pagina de ayda"
```

```
[main dc67466] Agrga pagina de ayda
 1 file changed, 1 insertion(+)
 create mode 100644 ayuda.php
```

Corrige el mensaje:

```bash
git commit --amend -m "Agrega página de ayuda"
```

```
[main 9cd6d10] Agrega página de ayuda
 Date: Fri Mar 13 11:00:00 2026 -0300
 1 file changed, 1 insertion(+)
 create mode 100644 ayuda.php
```

> 🤔 **Antes de ejecutarlo, piensa:** ¿cuántos commits relacionados con la página de ayuda vas a ver en el historial: uno o dos?

```bash
git log --oneline -2
```

(`-2` muestra solo los 2 últimos commits.)

<details>
<summary>Ver lo que deberías ver</summary>

```
9cd6d10 Agrega página de ayuda
41af074 Agrega .gitignore para ignorar archivos log y tmp
```

Uno solo. El commit con errores (`dc67466`) fue **reemplazado** por uno nuevo (`9cd6d10`, con otro hash). No quedó rastro del mensaje mal escrito.

</details>

Si en vez del mensaje te olvidaste de incluir un archivo, se hace así: `git add archivo-olvidado` y después `git commit --amend --no-edit` (`--no-edit` mantiene el mensaje como estaba).

> ⚠️ **Regla importante:** usa `--amend` solo con commits que **todavía no subiste a GitHub** (lo vemos en el tema 7). Como `--amend` reemplaza el commit, si ya lo subiste vas a tener problemas para volver a subir. Para commits ya subidos, usa `git revert` (sección 5).

## 4. Ver y recuperar versiones anteriores

Cada commit guarda una foto completa de tus archivos en ese momento. Puedes mirar cualquiera de esas fotos.

### ✍️ Tu turno: ver la primera versión de hola.php

Busca el hash de tu primer commit (el de más abajo):

```bash
git log --oneline
```

Para ver cómo era `hola.php` en ese commit, usa `git show hash:archivo` (con tu hash):

```bash
git show 3a166d3:hola.php
```

```
<?php echo "Hola mundo"; ?>
```

Esto solo **muestra** el contenido: no cambia nada en tu carpeta.

Si además quieres **traer** esa versión vieja a tu carpeta, usa `git restore --source`:

```bash
git restore --source=3a166d3 hola.php
cat hola.php
```

```
<?php echo "Hola mundo"; ?>
```

Ahora `hola.php` tiene el contenido viejo, y Git lo ve como un cambio más (`git status` lo muestra como `modified`). Si quisieras quedarte con esa versión, harías `git add` y `git commit`, como con cualquier cambio.

En este caso no la queremos. Vuelve a la versión del último commit con lo que aprendiste en la sección 1:

```bash
git restore hola.php
cat hola.php
```

```
<?php echo "Hola mundo"; echo "Adiós mundo"; echo "Versión 3"; ?>
```

## 5. Deshacer un commit viejo: git revert

¿Y si el error está en un commit que ya hiciste, quizás hace tiempo, y quizás ya compartido? `git revert` crea un **commit nuevo que hace lo contrario** del commit que le indicas. Si el commit agregó una línea, el revert la quita; si creó un archivo, el revert lo borra.

El commit original **no se borra**: el historial muestra las dos cosas, el error y su corrección. Por eso es la forma segura de deshacer commits que ya subiste a GitHub.

### ✍️ Tu turno: deshacer un commit

Crea un aviso y guárdalo en un commit:

```bash
echo '<?php echo "Sitio en mantenimiento"; ?>' > aviso.php
git add aviso.php
git commit -m "Agrega aviso de mantenimiento"
```

Ahora decides que el aviso fue un error. Revierte ese commit:

```bash
git revert --no-edit HEAD
```

- `HEAD` significa "el último commit". En su lugar puedes poner el hash de cualquier commit.
- `--no-edit` usa el mensaje automático, sin abrir el editor.

```
[main e0b6e16] Revert "Agrega aviso de mantenimiento"
 1 file changed, 1 deletion(-)
 delete mode 100644 aviso.php
```

> 🤔 **Antes de ejecutarlo, piensa:** ¿`aviso.php` sigue en la carpeta? ¿Y el commit "Agrega aviso de mantenimiento" sigue en el historial?

```bash
ls
git log --oneline -3
```

<details>
<summary>Ver lo que deberías ver</summary>

```
ayuda.php  contacto.php  hola.php
```

```
e0b6e16 Revert "Agrega aviso de mantenimiento"
783aefc Agrega aviso de mantenimiento
9cd6d10 Agrega página de ayuda
```

`aviso.php` ya no está en la carpeta, pero el commit que lo creó **sigue en el historial**, seguido del commit que lo deshizo. Si algún día lo necesitas, lo puedes recuperar con lo que viste en la sección 4.

</details>

> **Un comando que vas a ver en internet: `git reset --hard`.** También deshace commits, pero borrando historial y cambios sin guardar, sin pedir confirmación. Es fácil perder trabajo con él. Mientras estés aprendiendo, usa los comandos de este tema.

## 🧩 Desafío

Borraste `hola.php` por accidente:

```bash
rm hola.php
```

Recupéralo **sin volver a escribir su contenido**. Pista: empieza por preguntarle a Git qué ve.

<details>
<summary>Ver una solución</summary>

```bash
git status
```

Git muestra el archivo como `deleted: hola.php`, y la sugerencia de siempre: `use "git restore <file>..." to discard changes`. Borrar un archivo es un cambio más, así que se descarta igual que cualquier otro:

```bash
git restore hola.php
ls
```

```
ayuda.php  contacto.php  hola.php
```

Cualquier archivo que esté en un commit se puede recuperar así. Por eso conviene hacer commits seguido: lo que nunca estuvo en un commit, Git no lo puede devolver.

</details>

## ✅ Comprueba que todo salió bien

```bash
git log --oneline
```

Tienen que aparecer 8 commits:

```
e0b6e16 Revert "Agrega aviso de mantenimiento"
783aefc Agrega aviso de mantenimiento
9cd6d10 Agrega página de ayuda
41af074 Agrega .gitignore para ignorar archivos log y tmp
625e85b Agrega tercera línea con versión 3
a05d20c Agrega página de contacto
2ec05a4 Agrega segunda línea con despedida
3a166d3 Agrega archivo hola.php con saludo inicial
```

```bash
ls
```

```
ayuda.php  contacto.php  hola.php
```

Y `git status` tiene que decir `nothing to commit, working tree clean`. Si quedó algo pendiente (por ejemplo, `borrador.php` o un cambio en `hola.php`), ya sabes cómo descartarlo.

---

**Siguiente:** [Tema 6: Ramas y conflictos](06_ramas.md)
