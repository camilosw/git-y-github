# Tema 2: Tu primer repositorio — git init y git status

> **Objetivo:** crear un repositorio Git y entender qué es la carpeta `.git` y el comando `git status`.
>
> **Antes de empezar:** tienes que tener Git instalado y configurado ([tema 1](01_que_es_git.md)).

## ¿Qué es un repositorio?

Un repositorio (o "repo") es una carpeta cuyos cambios Git va registrando. Cualquier carpeta puede convertirse en un repositorio.

Piénsalo como una carpeta normal pero con superpoderes: Git recuerda cada cambio que guardes en los archivos que están dentro.

## git init: convertir una carpeta en repositorio

Para convertir una carpeta en repositorio se usa `git init`. Este comando crea dentro una carpeta oculta llamada `.git`.

### ✍️ Tu turno: crear el repositorio

Crea una carpeta para el proyecto, entra en ella e inicializa el repositorio. Hazlo dentro de tu carpeta personal de Ubuntu (`~`), no en `/mnt/c/...`: trabajar en el disco de Windows desde WSL es más lento y puede dar problemas.

```bash
cd ~
mkdir mi-proyecto
cd mi-proyecto
git init
```

Deberías ver (la ruta va a ser otra):

```
Initialized empty Git repository in /home/usuario/mi-proyecto/.git/
```

> Vas a usar esta carpeta `mi-proyecto` hasta el tema 9. Cada vez que abras la terminal para seguir el curso, entra primero en ella con `cd mi-proyecto`.

## La carpeta .git

La carpeta `.git` contiene todo el historial y la configuración del repositorio. Hay dos cosas importantes que saber:

- **No hay que tocarla ni borrarla.** Si la borras, pierdes todo el historial.
- **Es oculta.** No aparece con `ls`, pero sí con `ls -a`, que muestra también los archivos ocultos.

### ✍️ Tu turno: ver la carpeta oculta

```bash
ls -a
```

```
.  ..  .git
```

(`.` y `..` aparecen siempre: representan la carpeta actual y la carpeta de arriba.)

## git status: ver el estado del repositorio

`git status` es el comando que más vas a usar. Muestra el estado actual del repositorio: qué archivos cambiaron, cuáles están listos para guardarse, etc. Ante la duda, ejecuta `git status`.

### ✍️ Tu turno: crear un archivo y mirar el estado

Crea un archivo PHP simple. Puedes hacerlo con un editor de texto o con este comando:

```bash
echo '<?php echo "Hola mundo"; ?>' > hola.php
```

> 🤔 **Antes de ejecutarlo, piensa:** acabas de crear un archivo, pero no le dijiste nada a Git. ¿Crees que `git status` lo va a mostrar? ¿Va a decir que lo está cuidando?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hola.php

nothing added to commit but untracked files present (use "git add" to track)
```

Git **ve** el archivo, pero lo lista como "Untracked": todavía no lo está siguiendo.

</details>

> **¿Dice `On branch master` en vez de `On branch main`?** Te faltó el comando `git config --global init.defaultBranch main` del tema 1. Ejecútalo ahora y, para corregir este repositorio, ejecuta también `git branch -m main`.

## ¿Qué significa "Untracked files"?

Que un archivo esté "untracked" (sin seguimiento) significa que Git lo ve en la carpeta pero todavía no lo está siguiendo. Es como si Git dijera: "sé que este archivo existe, pero no me pediste que lo cuide".

Para que Git empiece a seguir un archivo hay que agregarlo con `git add`. Eso lo hacemos en el próximo tema.

## Leer lo que dice Git

Fíjate que `git status` no solo describe el estado: también te sugiere qué hacer, entre paréntesis. Por ejemplo: `(use "git add <file>..." to include in what will be committed)`. Acostúmbrate a leer esos mensajes: muchas veces la respuesta a "¿y ahora qué hago?" está ahí.

## ✅ Comprueba que todo salió bien

Dentro de `mi-proyecto`, ejecuta `git status`. Tiene que aparecer `hola.php` en la sección "Untracked files", como en la salida de arriba.

---

**Siguiente:** [Tema 3: El ciclo básico](03_ciclo_basico.md)
