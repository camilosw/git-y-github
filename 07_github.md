# Tema 7: GitHub — Subir tu código a la nube

> **Objetivo:** crear un repositorio en GitHub y sincronizarlo con tu repositorio local.
>
> **Antes de empezar:** entra en `mi-proyecto`. Tiene que estar en la rama `main`, con `git status` en `working tree clean`. También necesitas tu cuenta de GitHub ([tema 1](01_que_es_git.md)).

## ¿Para qué sirve GitHub?

Hasta ahora todo lo que hiciste quedó en tu computadora. Si se rompe el disco, pierdes todo. GitHub es un sitio web donde puedes subir tus repositorios para:

- **Respaldar:** tu código queda a salvo en la nube.
- **Compartir:** otras personas pueden verlo.
- **Colaborar:** trabajar en equipo sobre el mismo proyecto.

Piensa en GitHub como un Google Drive especial para código. Tu carpeta local sigue existiendo y funcionando igual; GitHub guarda una copia que mantienes sincronizada con dos comandos: `git push` (subir) y `git pull` (bajar).

Una diferencia importante con Google Drive: **nada se sincroniza solo**. Tus commits quedan en tu computadora hasta que hagas `git push`.

Para conectar tu repositorio con GitHub hacen falta tres pasos:

1. Crear un repositorio en GitHub, desde la web.
2. Configurar la autenticación, para que GitHub sepa que eres tú.
3. Conectar y subir, con `git remote` y `git push`.

## Crear un repositorio en GitHub

### ✍️ Tu turno: crear mi-proyecto en GitHub

1. Con tu sesión iniciada en GitHub, haz clic en el botón **+** (arriba a la derecha) y elige **New repository**
2. En **Repository name** pon `mi-proyecto`
3. Deja la visibilidad en **Public**
4. **No marques** "Add a README file", ni elijas `.gitignore` ni licencia: ya tienes un repositorio local y vamos a subirlo tal cual
5. Haz clic en **Create repository**

GitHub te muestra una página con instrucciones y la dirección (URL) del repositorio. Déjala abierta: la vas a necesitar.

## Autenticación: demostrarle a GitHub que eres tú

GitHub necesita verificar tu identidad cada vez que subes código. Hay dos métodos. **Elige uno solo**, el que te resulte más cómodo.

### Método 1: HTTPS con token personal

GitHub no acepta tu contraseña normal desde la terminal. En su lugar se usa un **token personal**, que funciona como una contraseña especial.

#### ✍️ Tu turno: crear el token

1. En GitHub, haz clic en tu foto de perfil (arriba a la derecha) → **Settings**
2. En el menú de la izquierda, baja hasta **Developer settings**
3. Entra en **Personal access tokens** → **Tokens (classic)**
4. Haz clic en **Generate new token** → **Generate new token (classic)**
5. Pon un nombre descriptivo (por ejemplo: "Mi computadora")
6. En **Expiration** elige **90 days**
7. En **Select scopes**, marca la casilla **repo** (permite subir código)
8. Haz clic en **Generate token**
9. **Copia el token ahora y guárdalo en un lugar seguro.** GitHub lo muestra una sola vez; si lo pierdes, tienes que crear otro. Trátalo como una contraseña: no lo compartas ni lo pegues en ningún archivo del proyecto.

Cuando hagas `git push` y la terminal te pida datos:

- **Username:** tu nombre de usuario de GitHub
- **Password:** pega el token (no tu contraseña de GitHub). Al pegarlo no vas a ver nada en la pantalla; es normal.

### Método 2: SSH con clave

SSH usa un par de claves, una pública y una privada, para identificarte sin escribir nada cada vez. Lleva un poco más de trabajo la primera vez, pero después es más cómodo.

#### ✍️ Tu turno: configurar SSH

**1. Genera el par de claves** (con tu email):

```bash
ssh-keygen -t ed25519 -C "tu@email.com"
```

Te pregunta dónde guardar la clave: presiona **Enter** para aceptar la ubicación por defecto. Después te pide una contraseña para la clave (passphrase): puedes dejarla vacía presionando Enter dos veces.

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/usuario/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/usuario/.ssh/id_ed25519
Your public key has been saved in /home/usuario/.ssh/id_ed25519.pub
```

**2. Muestra la clave pública** (la que se sube a GitHub):

```bash
cat ~/.ssh/id_ed25519.pub
```

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... tu@email.com
```

Copia toda esa línea. El archivo **sin** `.pub` es tu clave privada: nunca la compartas.

**3. Agrega la clave en GitHub:**

1. En GitHub: foto de perfil → **Settings** → **SSH and GPG keys**
2. Haz clic en **New SSH key**
3. Pon un título descriptivo (por ejemplo: "Mi computadora")
4. Pega la clave pública
5. Haz clic en **Add SSH key**

**4. Verifica que funciona:**

```bash
ssh -T git@github.com
```

La primera vez aparece una pregunta como esta:

```
The authenticity of host 'github.com (...)' can't be established.
ED25519 key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Es normal: tu computadora nunca se conectó a GitHub y te pide confirmar. Escribe `yes` y presiona Enter. Después deberías ver:

```
Hi tu-usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

Si ves ese mensaje, SSH funciona.

## Conectar tu repositorio local con GitHub

`git remote add` le dice a tu repositorio local cuál es la dirección del repositorio en GitHub.

### ✍️ Tu turno: conectar

Dentro de `mi-proyecto`, ejecuta **uno** de estos comandos, según el método que elegiste. Reemplaza `tu-usuario` por tu nombre de usuario de GitHub.

```bash
# Si elegiste HTTPS:
git remote add origin https://github.com/tu-usuario/mi-proyecto.git

# Si elegiste SSH:
git remote add origin git@github.com:tu-usuario/mi-proyecto.git
```

- `origin` es el nombre que le damos a la copia en GitHub. Es una convención: casi siempre se usa `origin`.
- La URL es la dirección de tu repositorio en GitHub.

Verifica que quedó bien:

```bash
git remote -v
```

```
origin	https://github.com/tu-usuario/mi-proyecto.git (fetch)
origin	https://github.com/tu-usuario/mi-proyecto.git (push)
```

(Con SSH, la URL empieza con `git@github.com:`.)

## Subir el código: git push

### ✍️ Tu turno: el primer push

La primera vez se usa `-u`, que deja vinculada tu rama `main` con la `main` de GitHub:

```bash
git push -u origin main
```

Si usas HTTPS, aquí te pide usuario y token. Al terminar deberías ver algo parecido a esto (los números varían):

```
Enumerating objects: 30, done.
...
To https://github.com/tu-usuario/mi-proyecto.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Lo importante es la línea `* [new branch]  main -> main`. Recarga la página de tu repositorio en GitHub: tienen que aparecer tus archivos. Se subió **todo el historial**, no solo los archivos: los 12 commits, incluidos el revert y el merge del conflicto.

> **¿Te sale `error: src refspec main does not match any`?** Tu rama principal se llama `master` en vez de `main`. Ejecuta `git branch -m main` y vuelve a intentar.

## README.md: la portada de tu proyecto

`README.md` es un archivo especial: GitHub lo muestra como portada del repositorio, debajo de la lista de archivos. Se escribe en **Markdown**, un formato de texto simple: por ejemplo, una línea que empieza con `#` es un título.

### ✍️ Tu turno: crear, guardar y subir el README

Crea un archivo `README.md` con un editor, con este contenido:

```markdown
# Mi Proyecto

Este es mi primer proyecto con Git y GitHub.
```

o con estos comandos:

```bash
echo "# Mi Proyecto" > README.md
echo "" >> README.md
echo "Este es mi primer proyecto con Git y GitHub." >> README.md
```

Guárdalo en un commit:

```bash
git add README.md
git commit -m "Agrega README con descripción del proyecto"
```

> 🤔 **Antes de ejecutarlo, piensa:** hiciste un commit nuevo pero todavía no lo subiste. ¿Ya aparece el README en GitHub? ¿Qué va a decir `git status` sobre la relación entre tu `main` y la de GitHub?

```bash
git status
```

<details>
<summary>Ver lo que deberías ver</summary>

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

"Ahead by 1 commit" significa que tu `main` tiene 1 commit que GitHub todavía no tiene. Si recargas GitHub, el README no está. Los commits no se suben solos: falta el push.

</details>

Súbelo. Como el primer push ya vinculó las ramas, alcanza con `git push`:

```bash
git push
```

```
...
To https://github.com/tu-usuario/mi-proyecto.git
   f3ea06d..14892ce  main -> main
```

Recarga la página en GitHub: el README aparece como portada.

> **Recuerda la regla del tema 5:** ahora que tus commits están en GitHub, no uses `git commit --amend` con ellos. Para deshacer algo que ya subiste, usa `git revert`.

## Resumen de comandos nuevos

| Comando | Qué hace |
|---|---|
| `git remote add origin URL` | Conecta tu repositorio local con GitHub |
| `git remote -v` | Muestra los remotos configurados |
| `git push -u origin main` | Sube por primera vez y vincula la rama |
| `git push` | Sube los commits nuevos |

## 🧩 Desafío

Sin usar la terminal, encuentra **en la página de GitHub** el commit en el que resolviste el conflicto del tema 6, y mira qué cambió en `hola.php` en el commit "Cambia el saludo por buenos días".

<details>
<summary>Ver una solución</summary>

1. En la página del repositorio, haz clic en el texto que dice **commits** (con un ícono de reloj), arriba de la lista de archivos.
2. Vas a ver la lista de commits, como un `git log` con más diseño. Ahí está "Merge branch 'cambiar-saludo'".
3. Haz clic en el mensaje "Cambia el saludo por buenos días". GitHub muestra el diff, igual que `git show`: la línea vieja en rojo y la nueva en verde.

Todo lo que viste en la terminal con `git log` y `git show` también se puede explorar desde GitHub.

</details>

## ✅ Comprueba que todo salió bien

**En GitHub**, abre `https://github.com/tu-usuario/mi-proyecto` y verifica que:

- Aparecen los archivos: `.gitignore`, `README.md`, `ayuda.php`, `contacto.php`, `hola.php`, `nuevo.php`
- El contenido del README se ve debajo de la lista de archivos
- En la lista de commits aparecen tus 13 commits

**En la terminal:**

```bash
git status
```

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

"Up to date with 'origin/main'" significa que tu repositorio local y el de GitHub tienen los mismos commits.

---

**Siguiente:** [Tema 8: Issues y Pull Requests](08_colaboracion.md)
