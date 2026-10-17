# Git y GitHub desde cero

Este curso te enseña a usar **Git** para guardar el historial de tu código y **GitHub** para respaldarlo y trabajar con otras personas. Va desde lo más básico (qué es Git, cómo instalarlo) hasta proponer cambios con Pull Requests y combinar tu trabajo con el de una compañera.

Los ejemplos usan archivos PHP muy simples para que el foco esté en Git y no en el código. No hace falta saber PHP ni ejecutar los archivos.

## Cómo usar este curso

- **Sigue los temas en orden.** Cada uno se apoya en el anterior.
- **La práctica está dentro de la explicación.** Cuando veas un bloque **✍️ Tu turno**, ejecuta esos comandos en tu terminal antes de seguir leyendo. No hay ejercicios aparte: lo que lees es lo que haces.
- **Del tema 2 al 9 trabajas siempre en el mismo repositorio**, `mi-proyecto`. Cada tema continúa donde quedó el anterior. El tema 10 empieza un proyecto nuevo desde cero.
- **Al final de cada tema hay una sección ✅ Comprueba** con comandos para verificar que todo salió bien antes de pasar al siguiente.

### Pensar antes de ejecutar

Vas a encontrar dos tipos de pausas para pensar:

- **🤔 Antes de ejecutarlo, piensa:** una pregunta sobre qué va a pasar. Respóndela mentalmente (o en un papel) antes de ejecutar el comando, y después abre **"Ver lo que deberías ver"** para comparar. Equivocarse aquí es bueno: es justo lo que te enseña cómo funciona Git.
- **🧩 Desafío:** una tarea sin los comandos escritos. Intenta resolverla con lo que aprendiste. Si te atascas, la solución está escondida debajo.

### Sobre las salidas de los comandos

Después de cada comando se muestra lo que deberías ver en la terminal. Algunas partes van a ser distintas en tu computadora, y está bien:

- Los **códigos de los commits** (como `3a166d3`) son únicos: los tuyos van a ser otros.
- Las **fechas**, las **rutas de carpetas** y los **números** de las salidas de `git push` y `git pull` también cambian.
- El **orden** en que `ls` lista los archivos depende del sistema.

Lo importante es que coincidan los mensajes, los nombres de archivos y de ramas.

### ¿Qué terminal usar?

Este curso está pensado para **Windows con WSL** (Subsistema de Windows para Linux) y la distribución **Ubuntu**. Todos los comandos se ejecutan en la terminal de Ubuntu, no en PowerShell ni en CMD. Si todavía no tienes WSL, en el tema 1 verás cómo instalarlo.

## Temas

| Tema | Lo que aprendes |
| ---- | --------------- |
| [1. ¿Qué es Git?](01_que_es_git.md) | Para qué sirve, instalación, cuenta de GitHub y configuración |
| [2. Tu primer repositorio](02_primer_repositorio.md) | `git init`, `git status`, la carpeta `.git` |
| [3. El ciclo básico](03_ciclo_basico.md) | `git add`, `git commit`, para qué sirve el área de preparación |
| [4. Historial](04_historial.md) | `git log`, `git show`, `git diff`, `.gitignore` |
| [5. Deshacer cambios](05_deshacer.md) | `git restore`, `git commit --amend`, `git revert`, ver versiones anteriores |
| [6. Ramas y conflictos](06_ramas.md) | `git switch`, `git merge`, resolver un conflicto |
| [7. GitHub](07_github.md) | Autenticación, `git remote`, `git push`, README |
| [8. Issues y Pull Requests](08_colaboracion.md) | Organizar el trabajo y proponer cambios para revisar |
| [9. Trabajar en equipo](09_trabajo_en_equipo.md) | `git clone`, `git pull`, combinar tu trabajo con el de otra persona |
| [10. Proyecto integrador](10_proyecto_integrador.md) | Un proyecto completo, desde cero, usando todo |
