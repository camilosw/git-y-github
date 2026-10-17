# Tema 1: ¿Qué es Git? — Control de versiones

> **Objetivo:** entender qué es el control de versiones, por qué es útil, y dejar Git instalado y configurado en tu computadora.

## ¿Qué problema resuelve Git?

Imagina que estás escribiendo un informe y vas guardando archivos como `informe_v1.docx`, `informe_v2_final.docx`, `informe_v2_final_FINAL.docx`. Git resuelve este caos: guarda el historial de cambios por ti, con un mensaje que describe cada cambio. Así siempre puedes volver a cualquier versión anterior (cómo hacerlo lo vemos en el tema 5).

Es como tener un cuaderno donde anotas cada cambio que haces en tu proyecto, con la fecha y una descripción. Si algo sale mal, puedes mirar atrás y ver cómo estaba todo antes.

## Git y GitHub no son lo mismo

- **Git** es un programa que se instala en tu computadora. Funciona **localmente**: no necesitas internet para usarlo.
- **GitHub** es un sitio web donde puedes subir tu código para respaldarlo, compartirlo y trabajar con otras personas.

Hasta el tema 7 vamos a trabajar solo con Git, en tu computadora. Pero en este tema ya vas a crear tu cuenta de GitHub, porque la necesitas para elegir qué email usar.

## Preparar WSL

Este curso usa **WSL** (Windows Subsystem for Linux), que te da una terminal de Linux dentro de Windows. Necesitas Windows 10 (versión 2004 o posterior) o Windows 11.

### ✍️ Tu turno: instalar WSL con Ubuntu

Si ya tienes Ubuntu en WSL, salta a la sección "Instalar Git".

1. Abre **PowerShell** como administrador (clic derecho en el menú Inicio → **Terminal (administrador)** o **Windows PowerShell (administrador)**)
2. Ejecuta:

   ```powershell
   wsl --install -d Ubuntu
   ```

3. Reinicia el equipo si te lo pide
4. Abre **Ubuntu** desde el menú Inicio. La primera vez te pide crear un nombre de usuario y una contraseña de Linux (al escribir la contraseña no verás nada en pantalla; es normal)

A partir de ahora, **todo el curso se hace en esta terminal de Ubuntu**. Es la única vez que usarás PowerShell.

## Instalar Git

Primero fíjate si ya lo tienes. En la terminal de Ubuntu ejecuta:

```bash
git --version
```

Si aparece un número de versión, ya está instalado y puedes saltar a la sección "Tu turno: verificar la instalación". Si da error, instálalo:

```bash
sudo apt update
sudo apt install git
```

Te pide la contraseña de Linux que creaste al abrir Ubuntu por primera vez.

### ✍️ Tu turno: verificar la instalación

```bash
git --version
```

Deberías ver algo así (el número puede ser otro):

```
git version 2.43.0
```

Si ves un número de versión, Git está instalado. Este curso necesita la versión 2.23 o una más nueva.

## Crear tu cuenta de GitHub y elegir tu email

Git guarda tu nombre y tu email en cada cambio que registres. Cuando subas tu proyecto a GitHub (tema 7), cualquiera que lo mire va a poder ver ese email. Si no quieres que tu email personal quede a la vista, GitHub te da una **dirección privada** que sirve justamente para esto.

### ✍️ Tu turno: crear la cuenta

1. Entra a https://github.com y haz clic en **Sign up**
2. Elige un nombre de usuario, tu email y una contraseña
3. Verifica tu email (GitHub te manda un mensaje)

### ✍️ Tu turno: obtener tu email privado

1. Con tu sesión iniciada, haz clic en tu foto de perfil (arriba a la derecha) → **Settings**
2. En el menú de la izquierda, entra en **Emails**
3. Marca la opción **Keep my email addresses private**
4. Debajo aparece una dirección parecida a `12345678+tu-usuario@users.noreply.github.com`. Cópiala: es la que vas a usar en el próximo paso.

> Si no te importa que tu email se vea, puedes saltear esto y usar el mismo email con el que creaste la cuenta.

## Configuración inicial

Antes de usar Git tienes que decirle quién eres: tu nombre y tu email van a quedar en cada cambio que guardes, para que se sepa quién hizo qué.

También vamos a configurar el nombre de la **rama principal**. Las ramas las vemos en el tema 6. Por ahora alcanza con saber que todo proyecto tiene una rama principal y que en este curso (y en GitHub) se llama `main`. Algunas versiones de Git la llaman `master` si no le dices otra cosa, y eso te traería problemas al subir el código a GitHub.

### ✍️ Tu turno: configurar Git

Ejecuta estos tres comandos, con tu nombre y el email que elegiste:

```bash
# Tu nombre (aparecerá en cada cambio que guardes)
git config --global user.name "Tu Nombre"

# Tu email (el privado de GitHub, o el de tu cuenta)
git config --global user.email "12345678+tu-usuario@users.noreply.github.com"

# Que la rama principal de tus proyectos se llame "main"
git config --global init.defaultBranch main
```

Estos comandos no muestran nada si salen bien.

La opción `--global` significa que la configuración vale para todos tus proyectos en esta computadora. Solo hay que hacerlo una vez.

## ✅ Comprueba que todo salió bien

```bash
git config --list
```

Vas a ver una lista que puede ser larga. Busca que aparezcan estas tres líneas con tus datos:

```
user.name=Tu Nombre
user.email=12345678+tu-usuario@users.noreply.github.com
init.defaultbranch=main
```

También puedes consultar cada valor por separado:

```bash
git config user.name
git config user.email
```

```
Tu Nombre
12345678+tu-usuario@users.noreply.github.com
```

Si ves tus datos, está todo listo.

---

**Siguiente:** [Tema 2: Tu primer repositorio](02_primer_repositorio.md)
