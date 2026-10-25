---
description: SDD · Valida la spec RF por RF (consultas de solo lectura + comandos de verificación + verificación manual)
argument-hint: <NNNN_NombreFeature>
---

Fase `validate`. Carpeta: `.docs/002_specs/$1/`.

Qué lee el reviewer: el checklist "Hecho cuando" del paso o fase que se valida, y los criterios de aceptación de los RF que ese paso cita. No las tareas ni la spec completas. Los comandos de verificación, en `AGENTS.md` › Comandos.

Alcance: archivos citados en avance, plan y tareas, más los archivos nuevos sin seguimiento que liste `git status` (no `git diff`) y que estén en la carpeta del feature o citados. Todo otro archivo modificado o sin seguimiento se informa como "ajeno o no atribuible", sin revisarlo como propio. El usuario decide si algún ajeno pertenece al feature. Con seguimiento se usa su diferencia; en los nuevos, su contenido. Si un archivo mezcla cambios del feature y ajenos, se avisa. Sin Git, el alcance es solo lo citado.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **reviewer** la validación dentro de ese alcance:
   - Comandos de lint, sintaxis y prueba de `AGENTS.md` › Comandos en cada archivo del alcance.
   - Evidencia por RF con consultas de solo lectura o salida de pruebas.
   - Lista de verificaciones manuales de UI, con qué abrir, qué hacer y qué debe verse.
   - Clasifica el cambio y aplica el modo que declara `.agents/skills/seguridad-basica/SKILL.md`.
3. No arregles nada. Muestra el veredicto y la lista de verificaciones manuales. Si el veredicto es CAMBIOS NECESARIOS, se vuelve al implementer con la lista exacta y después otra vez al reviewer. Máximo 2 vueltas; si sigue fallando, se para y se explica qué ocurre.
4. Cuando se confirme cada verificación manual, anótala con su hora en Pruebas de `05_avance.md`. Sin eso, el RF queda pendiente.
5. Si se pide evidencia formal, la sesión escribe `07_pruebas_qa.md` y el harness de `tests/` con lo que el reviewer devuelve; el reviewer, en una segunda invocación, ejecuta el harness en modo lectura y devuelve el resultado; la sesión lo anota en `07_pruebas_qa.md`. Esa invocación cuenta como una de las 2 vueltas. Si una vuelta ya se gastó en CAMBIOS NECESARIOS, queda una para esa invocación. El comando no exige que el reviewer carezca de `Write` para seguir: con el reviewer anterior la sesión escribe igual y solo se pierde la garantía de la fila 1 (RF-21 b). El reviewer nuevo, aunque el comando que tenga delante sea el anterior, devuelve el contenido y no escribe esos archivos (RF-21 a).
6. La sesión principal registra la fila `validate` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-flujo $1` (o la corrección que toque).

Qué no hace: no corrige código. No lee las tareas ni la spec enteras. No revisa como propio lo "ajeno o no atribuible". No copia la regla de los dos alcances ni el formato de la fila de tiempos. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
