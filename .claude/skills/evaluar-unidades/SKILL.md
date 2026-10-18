---
name: evaluar-unidades
description: Evalúa de forma interactiva las unidades del curso de Git y GitHub (archivos 01 a 10 de esta carpeta) con preguntas de opción múltiple, corrigiendo después de cada unidad. Úsalo cuando el usuario pida que lo evalúen, examinen o pongan a prueba sobre uno o más temas del curso.
argument-hint: "[rango de unidades, ej: 1-5, 3 o 2-4]"
---

# Evaluar unidades del curso

Evalúa al usuario sobre las unidades indicadas en `$ARGUMENTS`. Los temas son los archivos `NN_*.md` de la carpeta del curso (`01_que_es_git.md` a `10_proyecto_integrador.md`).

## 1. Determinar el rango

- Si `$ARGUMENTS` trae un rango (`1-5`) o una sola unidad (`3`), úsalo.
- Si está vacío, pregunta con `AskUserQuestion` qué unidades quiere que evalúes. Ofrece rangos habituales (por ejemplo `1-5`, `6-10`, `1-10`) y una opción para una sola unidad; el usuario puede usar "Other" para indicar un rango propio.
- Si el rango es inválido (fuera de 1-10, o el inicio es mayor que el final), dilo y vuelve a preguntar.

## 2. Preparar el material

Lee los archivos de las unidades del rango antes de preguntar. Basa las preguntas solo en lo que dicen esos archivos, no en conocimiento general de Git.

## 3. Hacer las preguntas

- Usa `AskUserQuestion`, con 4 preguntas de opción múltiple por unidad (una llamada por unidad).
- Cada pregunta lleva un `header` corto y empieza con la unidad, por ejemplo `U3 · ...`.
- Cada pregunta tiene 4 opciones, una sola correcta. Varía su posición entre preguntas. Los distractores deben ser errores plausibles (confusiones típicas del tema), no opciones absurdas.
- No marques ninguna opción como "(Recommended)" y no des pistas sobre cuál es la correcta.
- Mezcla tipos: conceptos, qué comando usar en una situación, y qué muestra Git ante cierto estado.

## 4. Corregir después de cada unidad

Al recibir las 4 respuestas de una unidad, corrígela antes de pasar a la siguiente:

- Para cada pregunta di si acertó, cuál era la respuesta correcta y explica en una o dos líneas por qué, citando el tema.
- Si falló alguna, indica qué sección del tema repasar.
- Muestra el puntaje parcial de la unidad (5 puntos por pregunta, sobre 20).

No reveles respuestas correctas mientras el usuario responde una unidad.

## 5. Cierre

Al terminar la última unidad del rango, entrega:

- Puntaje total (5 puntos por pregunta, sobre el máximo del rango evaluado).
- Desglose por unidad.
- Las unidades más débiles y qué repasar.
- Si aprobó: 70 % o más del puntaje máximo. Si el rango llega hasta la unidad N, di si puede avanzar a la unidad N+1 (o que terminó el curso si N es 10).
