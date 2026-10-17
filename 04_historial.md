# Tema 4: Historial y navegación — log, show, diff, gitignore

> **Objetivo:** recorrer el historial, ver qué cambió en cada commit y antes de guardar, e indicarle a Git qué archivos ignorar. Al terminar vas a tener 5 commits.
>
> **Antes de empezar:** entra en `mi-proyecto`. Tiene que tener los 3 commits del [tema 3](03_ciclo_basico.md).

## git log: ver el historial

Cada commit queda guardado en el historial, y `git log` lo muestra. De cada commit vas a ver:

- El **hash**: un identificador único, un código largo de números y letras (de la `a` a la `f`).
- El **autor**: el nombre y el email que configuraste en el tema 1.
- La **fecha**.
- El **mensaje** que escribiste.

### ✍️ Tu turno: mirar el historial

```bash
git log
```

Deberías ver algo así:

```
commit a05d20c84270a8e32d95e4343253df076e243e92 (HEAD -> main)
Author: Tu Nombre <12345678+tu-usuario@users.noreply.github.com>
Date:   Fri Mar 13 10:45:00 2026 -0300

    Agrega página de contacto

commit 2ec05a41bca50410a21f831959ebfa510df2833c
Author: Tu Nombre <12345678+tu-usuario@users.noreply.github.com>
Date:   Fri Mar 13 10:30:00 2026 -0300

    Agrega segunda línea con despedida

commit 3a166d3adfe5f04d426e2a95427f76e20cd10093
Author: Tu Nombre <12345678+tu-usuario@users.noreply.github.com>
Date:   Fri Mar 13 10:00:00 2026 -0300

    Agrega archivo hola.php con saludo inicial
```

Los commits aparecen del más reciente al más antiguo. `HEAD -> main` indica dónde estás parada: en la rama `main`, en su último commit.

> Si el historial es largo, `git log` lo muestra por páginas. Usa las flechas para moverte y la tecla `q` para salir.

## git log --oneline: formato compacto

Cuando hay muchos commits, el formato completo cuesta leerlo. Con `--oneline` ves una línea por commit:

```bash
git log --oneline
```

```
a05d20c Agrega página de contacto
2ec05a4 Agrega segunda línea con despedida
3a166d3 Agrega archivo hola.php con saludo inicial
```

Muestra solo el principio del hash, que alcanza para identificar cada commit, y el mensaje. Esos 7 caracteres son los que vas a usar cuando quieras referirte a un commit en otros comandos.

## git show: ver qué cambió en un commit

`git log` te dice **qué commits** hay. Para ver **qué cambió** en uno de ellos, usa `git show` seguido de su hash.

### ✍️ Tu turno: inspeccionar un commit

Copia el hash del commit "Agrega página de contacto" de **tu** `git log --oneline` y úsalo así:

```bash
git show a05d20c
```

```
commit a05d20c84270a8e32d95e4343253df076e243e92
Author: Tu Nombre <12345678+tu-usuario@users.noreply.github.com>
Date:   Fri Mar 13 10:45:00 2026 -0300

    Agrega página de contacto

diff --git a/contacto.php b/contacto.php
new file mode 100644
index 0000000..5da956e
--- /dev/null
+++ b/contacto.php
@@ -0,0 +1 @@
+<?php echo "Escríbenos a contacto@ejemplo.com"; ?>
```

Arriba están los datos del commit, y abajo los cambios: un archivo nuevo, `contacto.php`, con una línea agregada (la que empieza con `+`). Esa forma de mostrar cambios se llama **diff**, y la vemos a continuación.

## git diff: ver qué cambió antes de guardarlo

`git diff` muestra exactamente qué cambió en tus archivos **desde el último commit**, en los cambios que todavía no pasaste por `git add`. Es como comparar el "antes" y el "después".

### ✍️ Tu turno: revisar un cambio antes de guardarlo

Agrégale a `hola.php` un tercer `echo` con el editor, o ejecuta:

```bash
echo '<?php echo "Hola mundo"; echo "Adiós mundo"; echo "Versión 3"; ?>' > hola.php
```

Ahora mira qué cambió:

```bash
git diff
```

```
diff --git a/hola.php b/hola.php
index 3d8c88e..a3bb668 100644
--- a/hola.php
+++ b/hola.php
@@ -1 +1 @@
-<?php echo "Hola mundo"; echo "Adiós mundo"; ?>
+<?php echo "Hola mundo"; echo "Adiós mundo"; echo "Versión 3"; ?>
```

Las primeras cinco líneas son un encabezado que dice qué archivo se compara y en qué parte. Lo importante está abajo:

- Las líneas que empiezan con `-` (en rojo) son lo que **se quitó**.
- Las líneas que empiezan con `+` (en verde) son lo que **se agregó**.

Revisar el diff antes de cada commit te ayuda a confirmar que no metiste nada por error.

Ahora pasa el cambio a la caja:

```bash
git add hola.php
```

> 🤔 **Antes de ejecutarlo, piensa:** el cambio sigue sin estar en un commit. ¿Qué va a mostrar `git diff` ahora?

```bash
git diff
```

<details>
<summary>Ver lo que deberías ver</summary>

Nada. `git diff` solo muestra los cambios que **todavía no** pasaste por `git add`. Como ya está en la caja, no aparece.

Para ver lo que está en la caja y todavía no guardaste, usa:

```bash
git diff --staged
```

Vas a ver el mismo diff de antes.

</details>

Guarda el cambio:

```bash
git commit -m "Agrega tercera línea con versión 3"
```

## .gitignore: ignorar archivos

Hay archivos que no queremos que Git siga:

- Archivos de registro (`.log`): se generan solos y no son parte del código.
- Archivos temporales (`.tmp`).
- Carpetas como `vendor/` o `node_modules/`, que se pueden volver a generar.
- **Archivos con contraseñas o datos privados.** Estos nunca deben entrar en el historial.

Para decirle a Git qué ignorar, se crea un archivo llamado `.gitignore` en la carpeta principal del proyecto, con un patrón por línea.

### ✍️ Tu turno: crear un .gitignore

Crea el archivo `.gitignore` con un editor, con estas dos líneas:

```
*.log
*.tmp
```

o con estos comandos:

```bash
echo "*.log" > .gitignore
echo "*.tmp" >> .gitignore
```

(`>` crea el archivo, o lo reemplaza si existe; `>>` agrega una línea al final.)

Ahora prueba que funciona: crea un archivo `.log`.

```bash
echo "esto es un log de prueba" > prueba.log
```

> 🤔 **Antes de ejecutarlo, piensa:** en la carpeta hay dos archivos nuevos, `.gitignore` y `prueba.log`. ¿Cuáles va a listar `git status` como "Untracked"?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

Solo `.gitignore`. `prueba.log` no aparece: Git lo ignora gracias al patrón `*.log`. El `.gitignore`, en cambio, no se ignora a sí mismo: es parte del proyecto y hay que guardarlo.

</details>

Guarda el `.gitignore`:

```bash
git add .gitignore
git commit -m "Agrega .gitignore para ignorar archivos log y tmp"
```

Ya comprobaste que funciona, así que puedes borrar el archivo de prueba:

```bash
rm prueba.log
```

## Patrones comunes en .gitignore

```
# Las líneas que empiezan con # son comentarios

# Todos los archivos .log
*.log

# Una carpeta completa
vendor/
node_modules/

# Un archivo específico
config_local.php
```

> **Importante:** `.gitignore` solo afecta a archivos que Git todavía no sigue. Si un archivo ya está en un commit, agregarlo después al `.gitignore` no lo saca del historial. Por eso conviene crear el `.gitignore` al principio de cada proyecto.

## 🧩 Desafío

En algún commit agregaste la línea `echo "Adiós mundo"`. Encuentra ese commit y muestra exactamente qué cambió en él, usando solo comandos de este tema.

<details>
<summary>Ver una solución</summary>

Primero buscas el commit en el historial compacto:

```bash
git log --oneline
```

El mensaje "Agrega segunda línea con despedida" parece el indicado. Copias su hash y lo inspeccionas:

```bash
git show 2ec05a4
```

En el diff vas a ver la línea vieja con `-` y la nueva, que ya incluye `echo "Adiós mundo"`, con `+`.

Fíjate que pudiste encontrarlo rápido gracias al mensaje del commit. Con mensajes como "cambios", habrías tenido que abrir commit por commit.

</details>

## ✅ Comprueba que todo salió bien

```bash
git log --oneline
```

Tienen que aparecer 5 commits (los códigos van a ser otros):

```
41af074 Agrega .gitignore para ignorar archivos log y tmp
625e85b Agrega tercera línea con versión 3
a05d20c Agrega página de contacto
2ec05a4 Agrega segunda línea con despedida
3a166d3 Agrega archivo hola.php con saludo inicial
```

Y `git status` tiene que decir `nothing to commit, working tree clean`.

---

**Siguiente:** [Tema 5: Deshacer cambios](05_deshacer.md)
