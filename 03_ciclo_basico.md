# Tema 3: El ciclo básico — add, commit, status

> **Objetivo:** aprender el flujo fundamental de Git: modificar archivos, prepararlos y guardar un punto en el historial. Al terminar vas a tener 3 commits.
>
> **Antes de empezar:** entra en tu carpeta `mi-proyecto` del [tema 2](02_primer_repositorio.md). Tiene que tener el archivo `hola.php` sin seguimiento.

## Los tres lugares donde están tus cambios

Git distingue tres lugares:

1. **Directorio de trabajo** (working directory): tus archivos, tal como los ves y editas.
2. **Área de preparación** (staging area): los cambios que elegiste incluir en el próximo guardado.
3. **Repositorio** (historial): los guardados que ya hiciste. Cada guardado se llama **commit**.

### La analogía del paquete

Piénsalo como preparar un paquete para enviar:

- Tienes cosas sobre la mesa (directorio de trabajo): tus archivos modificados.
- Pones en la caja las que quieres mandar (área de preparación, con `git add`).
- Cierras y etiquetas la caja (historial, con `git commit`): esos cambios quedan registrados.

Este ciclo se repite todo el tiempo: editar archivos, `git add`, `git commit`.

## git add: preparar cambios

`git add` pasa los cambios de un archivo al área de preparación. Le estás diciendo a Git: "quiero incluir esto en el próximo commit".

### ✍️ Tu turno: preparar hola.php

```bash
git add hola.php
git status
```

Deberías ver:

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   hola.php
```

Ahora dice "Changes to be committed" en vez de "Untracked files" (si tu terminal tiene colores, el archivo aparece en verde). El archivo está en la caja, listo para guardarse.

## git commit: guardar en el historial

`git commit` toma todo lo que está en el área de preparación y lo guarda en el historial. Siempre lleva un mensaje que describe qué se hizo, que se escribe después de `-m`.

### ✍️ Tu turno: tu primer commit

```bash
git commit -m "Agrega archivo hola.php con saludo inicial"
```

Deberías ver algo así:

```
[main (root-commit) 3a166d3] Agrega archivo hola.php con saludo inicial
 1 file changed, 1 insertion(+)
 create mode 100644 hola.php
```

El código `3a166d3` identifica este commit (el tuyo va a ser otro). Lo vemos mejor en el próximo tema.

> 🤔 **Antes de ejecutarlo, piensa:** guardaste todo lo que había en la caja. ¿Qué va a decir ahora `git status`?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
nothing to commit, working tree clean
```

"Working tree clean" significa que no hay cambios pendientes: todo está guardado.

</details>

> **¿Te olvidaste el `-m` y la terminal cambió a una pantalla rara?** Sin `-m`, Git abre un editor de texto para que escribas el mensaje. Para salir sin hacer el commit:
> - Si abajo ves `~` en varias líneas, es **Vim**: presiona `Esc`, escribe `:q!` y presiona Enter.
> - Si abajo ves atajos como `^X Exit`, es **nano**: presiona `Ctrl+X`.
>
> Después vuelve a hacer el commit, esta vez con `-m "mensaje"`.

## ¿Para qué sirve el área de preparación?

Podrías preguntarte por qué hay un paso intermedio: ¿no sería más fácil guardar todo directamente? El área de preparación te deja **elegir qué cambios van juntos en cada commit**.

Imagina que en una misma tarde hiciste dos cosas que no tienen nada que ver: agregaste una despedida en `hola.php` y creaste una página de contacto. Lo ideal es guardarlas en dos commits separados, cada uno con su mensaje. Así el historial cuenta qué pasó, paso a paso, y si algún día quieres deshacer uno de los dos cambios, el otro no se ve afectado.

### ✍️ Tu turno: dos cambios, dos commits

Haz los dos cambios. Agrégale a `hola.php` un segundo `echo` que imprima "Adiós mundo" y crea un archivo `contacto.php`, con un editor o con estos comandos:

```bash
echo '<?php echo "Hola mundo"; echo "Adiós mundo"; ?>' > hola.php
echo '<?php echo "Escríbenos a contacto@ejemplo.com"; ?>' > contacto.php
```

Mira qué detectó Git:

```bash
git status
```

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   hola.php

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	contacto.php

no changes added to commit (use "git add" and/or "git commit -a")
```

Git ve dos cosas: `hola.php` fue modificado (`modified`) y `contacto.php` es nuevo (`Untracked`). Ninguno está todavía en la caja.

Ahora pon en la caja **solo** el cambio de `hola.php`:

```bash
git add hola.php
```

> 🤔 **Antes de ejecutarlo, piensa:** ¿cómo va a mostrar `git status` cada uno de los dos archivos ahora?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   hola.php

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	contacto.php
```

`hola.php` está en la caja ("Changes to be committed"); `contacto.php` sigue sobre la mesa.

</details>

Guarda el primer commit. Solo incluye lo que está en la caja:

```bash
git commit -m "Agrega segunda línea con despedida"
```

```
[main 2ec05a4] Agrega segunda línea con despedida
 1 file changed, 1 insertion(+), 1 deletion(-)
```

"1 insertion, 1 deletion" quiere decir que Git ve una línea vieja que se fue y una nueva que la reemplazó: así cuenta Git cualquier línea modificada.

Ahora el segundo cambio, en su propio commit:

```bash
git add contacto.php
git commit -m "Agrega página de contacto"
```

```
[main a05d20c] Agrega página de contacto
 1 file changed, 1 insertion(+)
 create mode 100644 contacto.php
```

## Buenos mensajes de commit

Un buen mensaje es breve y dice **qué** se hizo. La convención es empezar con un verbo: "Agrega…", "Corrige…", "Elimina…".

- "Agrega archivo de configuración" (bien)
- "Corrige error en el cálculo de precios" (bien)
- "Elimina código duplicado en funciones.php" (bien)
- "cambios" (mal: no dice nada)
- "actualización" (mal: es muy vago)
- "asdfgh" (muy mal)

Un truco: si para escribir el mensaje necesitas un "y" ("Agrega contacto **y** cambia el saludo"), probablemente eran dos commits.

## git add . : agregar todo de una vez

El punto (`.`) agrega **todos** los archivos nuevos y modificados de la carpeta:

```bash
git add .
```

Es práctico cuando cambiaste muchos archivos que van juntos, pero ten cuidado de no incluir archivos que no quieres guardar (archivos temporales, contraseñas, etc.). En el próximo tema vemos cómo evitarlo con `.gitignore`. Antes de un `git add .`, mira siempre `git status`.

## ✅ Comprueba que todo salió bien

```bash
git log --oneline
```

Tienen que aparecer exactamente 3 commits, el más reciente arriba (los códigos van a ser otros):

```
a05d20c Agrega página de contacto
2ec05a4 Agrega segunda línea con despedida
3a166d3 Agrega archivo hola.php con saludo inicial
```

Y `git status` tiene que decir `nothing to commit, working tree clean`.

---

**Siguiente:** [Tema 4: Historial](04_historial.md)
