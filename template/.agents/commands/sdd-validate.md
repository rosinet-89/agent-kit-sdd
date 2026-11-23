---
description: SDD · Valida la spec RF por RF (consultas de solo lectura + comandos de verificación + verificación manual)
argument-hint: <prefijo_NombreFeature>
---

Fase `validate`. Carpeta: `.docs/002_specs/<carpeta>/`.
Argumentos: $ARGUMENTS. La carpeta es su primera palabra. Regla: `.agents/README.md` › «Argumentos de un comando».

Qué lee el reviewer: el checklist "Hecho cuando" del paso o fase que se valida, y los criterios de aceptación de los RF que ese paso cita. No las tareas ni la spec completas. Los comandos de verificación, en `AGENTS.md` › Comandos.

Alcance: archivos citados en avance, plan y tareas, más los archivos nuevos sin seguimiento que liste `git status` (no `git diff`) y que estén en la carpeta del feature o citados. Todo otro archivo modificado o sin seguimiento se informa como "ajeno o no atribuible", sin revisarlo como propio. El usuario decide si algún ajeno pertenece al feature. Con seguimiento se usa su diferencia; en los nuevos, su contenido. Si un archivo mezcla cambios del feature y ajenos, se avisa. Sin Git, el alcance es solo lo citado.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Si el feature tiene `07_casos_prueba.md` (formato: `.agents/skills/sdd/plantillas.md` › `## 07_casos_prueba.md`), quien atiende la fase como sesión lanza el rol **tester** (dónde corre: `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol») con los casos ejecutables, antes del reviewer:
   - Antes de lanzarlo, se comprueba el encargo según `.agents/skills/sdd/restricciones-de-rol.md` › «Roles Haiku». Si no entrega, rige `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol».
   - Modo seguro por omisión: `tester` solo ejecuta casos que no escriben datos ni usan la red; el resto queda `manual` con el motivo.
   - La sesión anota en las corridas de `07_casos_prueba.md` lo que `tester` devuelve por caso (`obtenido`, `bloqueado` o `manual`).
   - La sesión marca `ok` o `falla` solo cuando la comparación entre lo esperado y lo obtenido es literal. Si requiere juicio, la decide el reviewer.
   - Los resultados y las confirmaciones manuales de la persona se anotan solo en ese documento; el avance lleva un enlace. Sin documento, se anotan en el avance.
   - La corrida de `tester` y la del reviewer forman una sola vuelta del tope de 2.
3. Ejecuta el rol **reviewer** (dónde corre: `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol») para validar dentro de ese alcance:
   - Su veredicto usa las corridas de `tester` y decide los `ok` o `falla` que requieren juicio.
   - Comandos de lint, sintaxis y prueba de `AGENTS.md` › Comandos en cada archivo del alcance.
   - Evidencia por RF con consultas de solo lectura o salida de pruebas.
   - Lista de verificaciones manuales de UI, con qué abrir, qué hacer y qué debe verse.
   - Clasifica el cambio y aplica el modo que declara `.agents/skills/seguridad-basica/SKILL.md`.
4. No arregles nada. Muestra el veredicto y la lista de verificaciones manuales. Si el veredicto es CAMBIOS NECESARIOS, se vuelve al implementer con la lista exacta y después otra vez al reviewer. Máximo 2 vueltas; si sigue fallando, se para y se explica qué ocurre.
5. Cuando se confirme cada verificación manual, anótala con su hora en Pruebas de `05_avance.md`. Sin eso, el RF queda pendiente.
6. Si se pide evidencia formal, o el feature ya tiene `07_casos_prueba.md`, la evidencia va en `07_casos_prueba.md` y el harness de solo lectura en `tests/`, que se conservan. El reviewer devuelve su contenido y no escribe esos archivos; la sesión los escribe. Si hace falta ejecutar el harness, el reviewer lo hace en una segunda invocación en modo lectura y devuelve el resultado, que la sesión anota en `07_casos_prueba.md`. Esa invocación cuenta como una de las 2 vueltas; si una vuelta ya se gastó en CAMBIOS NECESARIOS, queda una para ella. Sin `Write` en el reviewer, la sesión escribe igual. Compatibilidad: un `07_pruebas_qa.md` de un feature anterior se conserva sin renombrar y el reviewer lo lee como evidencia.
7. La sesión principal registra la fila `validate` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-flujo <carpeta>` (o la corrección que toque).

Qué no hace: no corrige código. No lee las tareas ni la spec enteras. No revisa como propio lo "ajeno o no atribuible". No copia la regla de los dos alcances ni el formato de la fila de tiempos. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
